<template>
  <!-- Women Banner Section Begin -->
  <section class="women-banner spad">
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-12 mt-5" v-if="products.length > 0">
          <carousel
            class="product-slider"
            :items="3"
            :autoplay="true"
            :nav="false"
            :dots="false"
            :loop="true"
          >
            <div class="product-item" v-for="itemProduct in products" v-bind:key="itemProduct.id">
              <div class="pi-pic">
                <img v-bind:src="itemProduct.galleries[0].image" alt />
                <ul>
                  <li class="w-icon active">
                    <a href="#" @click="saveKeranjang(itemProduct)">
                      <i class="icon_bag_alt"></i>
                    </a>
                  </li>
                  <li class="quick-view">
                    <router-link v-bind:to="/product/+itemProduct.slug">+ Quick View</router-link>
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <div class="catagory-name">{{itemProduct.type}}</div>
                <router-link to="/product">
                  <h5>{{itemProduct.name}}</h5>
                </router-link>
                <div class="product-price">
                  {{itemProduct.price}}
                  <span>$35.00</span>
                </div>
              </div>
            </div>
          </carousel>
        </div>
        <div class="col-lg-12" v-else>
          <p>Produk Terbaru Belum Tersedia</p>
        </div>
      </div>
    </div>
  </section>
  <!-- Women Banner Section End -->
</template>

<script>
// import banner slider owl carousel
import carousel from "vue-owl-carousel";
// import axios
import axios from "axios";
export default {
  name: "HeroProductMitrabakti",
  components: {
    carousel
  },
  data() {
    return {
      products: [],
      keranjangUser: []
    };
  },
  methods: {
    saveKeranjang(productDetails) {
      let memoriProduk = {
        id: productDetails.id,
        name: productDetails.name,
        price: productDetails.price,
        image: productDetails.galleries[0].image
      };
      this.keranjangUser.push(memoriProduk);
      // dataKucing diserialisasi menjadi string JSON
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);
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
    axios
      .get("http://127.0.0.1:8000/api/products")
      .then(result => (this.products = result.data.data.data))
      .catch(err => console.log(err));
  }
};
</script>

<style scoped>
.product-item {
  margin-right: 25px;
}
</style>
