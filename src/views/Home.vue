<template>
  <div class="home">
    <HeaderMitrabakti />
    <HeroMitrabakti />
    <HeroProductMitrabakti />
    <InstagramMitrabakti />
    <PartnerMitrabakti />
    <FooterMitrabakti />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderMitrabakti from "@/components/HeaderMitrabakti.vue";
import HeroMitrabakti from "@/components/HeroMitrabakti.vue";
import HeroProductMitrabakti from "@/components/HeroProductMitrabakti.vue";
import InstagramMitrabakti from "@/components/InstagramMitrabakti.vue";
import PartnerMitrabakti from "@/components/PartnerMitrabakti.vue";
import FooterMitrabakti from "@/components/FooterMitrabakti.vue";

export default {
  name: "Home",
  components: {
    HeaderMitrabakti,
    HeroMitrabakti,
    HeroProductMitrabakti,
    InstagramMitrabakti,
    PartnerMitrabakti,
    FooterMitrabakti,
  },
};
</script>
