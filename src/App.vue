<template>
  <LazyPdfVuer :options="pdfOptions" offscreenPages="1" v-on:pageChanged="onPageChanged" />
</template>

<script>
import LazyPdfVuer from "./components/LazyPdfVuer.vue";

export default {
  components: {
    LazyPdfVuer
  },
  computed: {
    pdfOptions() {
      return {
        url: "./sample.pdf",
        httpHeaders: {
          Authorization: "Bearer D6a28BjVtdS0D7jUJhdj-SZzipibN2lz1dEleR4fdyWqKy4v4shqPQcVix66v"
        }
      };
    }
  },
  methods: {
    onPageChanged({ currentPage, numPages }) {
      console.log("Page", currentPage + "/" + numPages);
    }
  }
};
</script>