<template>
  <!-- Header Section Begin -->
  <header class="header-section">
    <div class="header-top">
      <div class="container">
        <div class="ht-left">
          <div class="mail-service">
            <i class="fa fa-envelope"></i> hello.shayna@gmail.com
          </div>
          <div class="phone-service">
            <i class="fa fa-phone"></i> +628 22081996
          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="inner-header">
        <div class="row">
          <div class="col-lg-2 col-md-2">
            <div class="logo">
              <router-link to="/">
                <img src="img/logo_website_shayna.png" alt />
              </router-link>
            </div>
          </div>
          <div class="col-lg-7 col-md-7"></div>
          <div class="col-lg-3 text-right col-md-3">
            <ul class="nav-right">
              <li class="cart-icon">
                Keranjang Belanja &nbsp;
                <a href="#">
                  <i class="icon_bag_alt"></i>
                  <span>{{keranjangUser.length}}</span>
                </a>
                <div class="cart-hover">
                  <div class="select-items">
                    <table>
                      <tbody v-if="keranjangUser.length > 0">
                        <tr v-for="keranjang in keranjangUser" :key="keranjang.id">
                          <td class="si-pic">
                            <img :src="keranjang.image" class="photo-item" />
                          </td>
                          <td class="si-text">
                            <div class="product-selected">
                              <p>Rp. {{keranjang.price}} x {{keranjang.id}} - {{keranjang.index}}</p>
                              <h6>{{keranjang.name}}</h6>
                            </div>
                          </td>
                          <td @click="removeItem(keranjang.id)" class="si-close">
                            <i class="ti-close"></i>
                          </td>
                        </tr>
                      </tbody>
                      <tbody v-else>
                        <tr>
                          <td>Keranjang Kosong</td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                  <div class="select-total">
                    <span>total:</span>
                    <h5>Rp {{totalHarga}}</h5>
                  </div>
                  <div class="select-button">
                    <router-link to="/cart" class="primary-btn view-card">VIEW CARD</router-link>
                    <router-link href="#" class="primary-btn checkout-btn">CHECK OUT</router-link>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </header>
  <!-- Header End -->
</template>

<script>
export default {
  name: "HeaderMitrabakti",
  data() {
    return {
      keranjangUser: []
    };
  },
  methods: {
    removeItem(xx) {
      // this.keranjangUser.splice(index, 1);
      // const parsed = JSON.stringify(this.keranjangUser);
      // localStorage.setItem("keranjangUser", parsed);
      // window.location.reload();
      let faveGifs = JSON.parse(localStorage.getItem("keranjangUser"));
      let faveGif = faveGifs.map(faveGif => faveGif.id);
      let index = faveGif.findIndex(id => id == xx);
      this.keranjangUser.splice(index, 1);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);
      // window.location.reload();
      // eslint-disable-next-line no-console
      console.log(index);
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
    }
  }
};
</script>

<style scoped>
.photo-item {
  width: 80px;
  height: 80px;
}
</style>