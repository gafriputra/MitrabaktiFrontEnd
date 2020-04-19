<template>
  <div class="product">
    <HeaderMitrabakti />
    <FooterMitrabakti />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderMitrabakti from "@/components/HeaderMitrabakti.vue";
import FooterMitrabakti from "@/components/FooterMitrabakti.vue";

export default {
  name: "Product",
  components: {
    HeaderMitrabakti,
    FooterMitrabakti,
  },
};
</script>
