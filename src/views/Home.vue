<template>
  <div class="home">
    <HeaderMitrabakti />
    <HeroMitrabakti />
    <HeroProductMitrabakti />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderMitrabakti from "@/components/HeaderMitrabakti.vue";

import HeroMitrabakti from "@/components/HeroMitrabakti.vue";
import HeroProductMitrabakti from "@/components/HeroProductMitrabakti.vue";

export default {
  name: "Home",
  components: {
    HeaderMitrabakti,
    HeroMitrabakti,
    HeroProductMitrabakti,
  },
};
</script>
