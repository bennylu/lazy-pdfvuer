<template>
  <div id="app">
    <LazyPdfVuer path="./sample.pdf" />
  </div>
</template>

<script>
import LazyPdfVuer from "./components/LazyPdfVuer.vue";

export default {
  name: "App",
  components: {
    LazyPdfVuer
  }
};
</script>