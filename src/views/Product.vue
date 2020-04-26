<template>
  <div class="product">
    <HeaderMitrabakti />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/">
                <i class="fa fa-home"></i> Home
              </router-link>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="gambar_utama" alt />
                </div>
                <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                  <carousel
                    class="product-thumbs-track ps-slider"
                    :items="3"
                    :autoplay="true"
                    :nav="false"
                    :dots="false"
                    :loop="true"
                  >
                    <div
                      v-for="image in productDetails.galleries"
                      :key="image.id"
                      class="pt"
                      @click="changeImage(image.image)"
                      :class="image.image == gambar_utama ? 'active' : false"
                    >
                      <img :src="image.image" alt />
                    </div>
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{productDetails.type}}</span>
                    <h3>{{productDetails.name}}</h3>
                  </div>
                  <div class="pd-desc">
                    <p v-html="productDetails.description"></p>
                    <h4>Rp. {{productDetails.price}}</h4>
                  </div>
                  <div class="quantity">
                    <a
                      href="#"
                      @click="saveKeranjang(productDetails)"
                      class="primary-btn pd-cart"
                    >Add To Cart</a>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->
    <RelatedProductsMitrabakti />
    <FooterMitrabakti />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderMitrabakti from "@/components/HeaderMitrabakti.vue";
import RelatedProductsMitrabakti from "@/components/RelatedProductsMitrabakti.vue";
import FooterMitrabakti from "@/components/FooterMitrabakti.vue";
// carousel
import carousel from "vue-owl-carousel";
// axios
import axios from "axios";

export default {
  name: "Product",
  components: {
    HeaderMitrabakti,
    RelatedProductsMitrabakti,
    FooterMitrabakti,
    carousel
  },
  data() {
    return {
      gambar_utama: "",
      productDetails: [],
      keranjangUser: []
    };
  },
  methods: {
    changeImage(urlImage) {
      this.gambar_utama = urlImage;
    },
    setDataPicture(data) {
      // replace data product dengan API
      this.productDetails = data;
      //replace value gambar detail dengan dara dari API (galleries)
      this.gambar_utama = data.galleries[0].image;
    },
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
      .get("http://127.0.0.1:8000/api/products", {
        params: {
          slug: this.$route.params.slug
        }
      })
      .then(result => this.setDataPicture(result.data.data))
      // eslint-disable-text-line no-console;
      .catch(err => console.log(err));
  }
};
</script>

<style scoped>
.product-thumbs .pt {
  margin-right: 10px;
}
</style>
