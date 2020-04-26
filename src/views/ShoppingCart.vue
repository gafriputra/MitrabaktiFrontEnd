<template>
  <div id="shopping">
    <HeaderMitrabakti />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
      <div class="container">
        <div class="row">
          <div class="col-lg-12 text-left">
            <div class="breadcrumb-text product-more">
              <router-link to="/">
                <i class="fa fa-home"></i> Home
              </router-link>
              <span>Shopping Cart</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
      <div class="container">
        <div class="row">
          <div class="col-lg-8">
            <div class="row">
              <div class="col-lg-12">
                <div class="cart-table">
                  <table>
                    <thead>
                      <tr>
                        <th>Image</th>
                        <th class="p-name text-center">Product Name</th>
                        <th>Price</th>
                        <th>Action</th>
                      </tr>
                    </thead>
                    <tbody v-if="keranjangUser.length > 0">
                      <tr v-for="keranjang in keranjangUser" :key="keranjang.id">
                        <td class="cart-pic first-row">
                          <img class="img-cart" :src="keranjang.image" />
                        </td>
                        <td class="cart-title first-row text-center">
                          <h5>{{keranjang.name}}</h5>
                        </td>
                        <td class="p-price first-row">Rp. {{keranjang.price}}</td>
                        <td class="delete-item">
                          <a href="#" @click="removeItem(keranjang.index)">
                            <i class="material-icons">close</i>
                          </a>
                        </td>
                      </tr>
                    </tbody>
                    <tbody v-else>
                      <tr>
                        <td colspan="4">Keranjang Kosong</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
              <div class="col-lg-8 text-left" v-if="keranjangUser.length > 0">
                <h4 class="mb-4">Informasi Pembeli:</h4>
                <div class="user-checkout">
                  <form>
                    <div class="form-group">
                      <label for="namaLengkap">Nama lengkap</label>
                      <input
                        type="text"
                        class="form-control"
                        id="namaLengkap"
                        aria-describedby="namaHelp"
                        placeholder="Masukan Nama"
                        v-model="customerInfo.name"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">Email Address</label>
                      <input
                        type="email"
                        class="form-control"
                        id="emailAddress"
                        aria-describedby="emailHelp"
                        placeholder="Masukan Email"
                        v-model="customerInfo.email"
                      />
                    </div>
                    <div class="form-group">
                      <label for="namaLengkap">No. HP</label>
                      <input
                        type="text"
                        class="form-control"
                        id="noHP"
                        aria-describedby="noHPHelp"
                        placeholder="Masukan No. HP"
                        v-model="customerInfo.phone"
                      />
                    </div>
                    <div class="form-group">
                      <label for="alamatLengkap">Alamat Lengkap</label>
                      <textarea
                        class="form-control"
                        id="alamatLengkap"
                        rows="3"
                        v-model="customerInfo.address"
                      ></textarea>
                    </div>
                  </form>
                </div>
              </div>
            </div>
          </div>
          <div class="col-lg-4" v-if="keranjangUser.length > 0">
            <div class="row">
              <div class="col-lg-12">
                <div class="proceed-checkout text-left">
                  <ul>
                    <li class="subtotal">
                      ID Transaction
                      <span>#SH12000</span>
                    </li>
                    <li class="subtotal mt-3">
                      Subtotal
                      <span>Rp. {{totalHarga}}</span>
                    </li>
                    <li class="subtotal mt-3">
                      Pajak
                      <span>10%</span>
                    </li>
                    <li class="subtotal mt-3">
                      Total Biaya
                      <span>Rp. {{totalHargaPajak}}</span>
                    </li>
                    <li class="subtotal mt-3">
                      Bank Transfer
                      <span>Mandiri</span>
                    </li>
                    <li class="subtotal mt-3">
                      No. Rekening
                      <span>2208 1996 1403</span>
                    </li>
                    <li class="subtotal mt-3">
                      Nama Penerima
                      <span>{{customerInfo.name}}</span>
                    </li>
                  </ul>
                  <a @click="checkout()" href="#" class="proceed-btn">I ALREADY PAID</a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Shopping Cart Section End -->
  </div>
</template>

<script>
import HeaderMitrabakti from "@/components/HeaderMitrabakti.vue";
import Axios from "axios";

export default {
  name: "Cart",
  components: {
    HeaderMitrabakti
  },
  data() {
    return {
      keranjangUser: [],
      customerInfo: {
        name: "",
        email: "",
        phone: "",
        address: ""
      }
    };
  },
  methods: {
    removeItem(listProduk) {
      this.keranjangUser.splice(listProduk, 1);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);
    },
    checkout() {
      let productId = this.keranjangUser.map(function(product) {
        return product.id;
      });

      let checkoutData = {
        name: this.customerInfo.name,
        email: this.customerInfo.email,
        phone: this.customerInfo.phone,
        address: this.customerInfo.address,
        transaction_total: this.totalHargaPajak,
        transaction_status: "PENDING",
        transaction_details: productId
      };
      Axios.post("http://127.0.0.1:8000/api/checkout", checkoutData)
        .then(() => this.$router.push("success"))
        // eslint-disable-next-line no-console
        .catch(err => console.log(err));
    }
  },
  mounted() {
    if (localStorage.getItem("keranjangUser")) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
      } catch (e) {
        localStorage.removeItem("keranjangUser");
      }
    }
  },
  computed: {
    totalHarga() {
      return this.keranjangUser.reduce(function(items, data) {
        return items + data.price;
      }, 0);
    },
    totalHargaPajak() {
      let totalHargaPajak = this.totalHarga;
      return totalHargaPajak + totalHargaPajak * 0.1;
    }
  }
};
</script>
<style scoped>
.img-cart {
  width: 100px;
  height: 100px;
}
</style>