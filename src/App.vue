<template>
  <LazyPdfVuer path="./sample.pdf" offscreenPages="1" v-on:pageChanged="onPageChanged" />
</template>

<script>
import LazyPdfVuer from "./components/LazyPdfVuer.vue";

export default {
  components: {
    LazyPdfVuer
  },
  methods: {
    onPageChanged({ currentPage, numPages }) {
      console.log("Page", currentPage + "/" + numPages);
    }
  }
};
</script>