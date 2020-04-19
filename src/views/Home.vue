<template>
  <div class="home">
    <HeaderMitrabakti />
    <HeroMitrabakti />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderMitrabakti from "@/components/HeaderMitrabakti.vue";

import HeroMitrabakti from "@/components/HeroMitrabakti.vue";

export default {
  name: "Home",
  components: {
    HeaderMitrabakti,
    HeroMitrabakti,
  },
};
</script>
