<template>
  <LazyPdfVuer
    path="./sample.pdf"
    offscreenPages="1"
    v-on:pageChanged="onPageChanged"
    v-on:numPages="onNumPages"
  />
</template>

<script>
import LazyPdfVuer from "./components/LazyPdfVuer.vue";

export default {
  components: {
    LazyPdfVuer
  },
  methods: {
    onNumPages(pages) {
      console.log("Num pages", pages);
    },
    onPageChanged(page) {
      console.log("Page", page);
    }
  }
};
</script>