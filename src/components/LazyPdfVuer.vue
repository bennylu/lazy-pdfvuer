<template>
  <div v-if="pdfdata">
    <template v-for="i in numPages">
      <template v-if="i >= currentPage - Number(offscreenPages) && i <= currentPage + Number(offscreenPages)">
        <pdfvuer class="page" :style="pageStyle" :src="pdfdata" :page="i" :key="i" :id="i"></pdfvuer>
      </template>
      <template v-else>
        <div class="page" :style="pageStyle" :key="i" :id="i"></div>
      </template>
    </template>
  </div>
</template>

<style scoped>
.page {
  margin-bottom: 20px;
}
</style>
 
<script>
import pdfvuer from "pdfvuer";

export default {
  components: {
    pdfvuer
  },
  props: {
    options: Object,
    offscreenPages: String
  },
  data: function() {
    return {
      pdfdata: undefined,
      currentPage: 1,
      numPages: 0,
      pageHeight: 0
    };
  },
  computed: {
    pageStyle: function() {
      if (this.pageHeight == 0) return "";
      return "height: " + this.pageHeight + "px";
    }
  },
  mounted: function() {
    console.log(this.options);
    this.pdfdata = pdfvuer.createLoadingTask(this.options);
    this.pdfdata.then(pdf => {
      let self = this;
      this.numPages = pdf.numPages;

      self.$emit("pageChanged", {
        currentPage: 1,
        numPages: this.numPages
      });

      window.addEventListener("scroll", function() {
        changePage();
      }, true);

      function changePage() {
        let yOffset = window.pageYOffset;
        // let yOffset = document.getElementsByClassName("content-container")[0].scrollTop;
        let previousPage = self.currentPage;
        let i = 1;

        do {
          if (yOffset >= document.getElementById(i).offsetTop
              && yOffset <= document.getElementById(i + 1).offsetTop) {
            self.currentPage = i;
          }
        } while (++i < self.numPages);

        if (yOffset >= document.getElementById(i).offsetTop)
          self.currentPage = i;

        if (self.currentPage != previousPage)
          self.$emit("pageChanged", {
            currentPage: self.currentPage,
            numPages: self.numPages
          });

        if (self.pageHeight == 0)
          self.pageHeight = document.getElementById(self.currentPage).offsetHeight;
      }
    });
  },
  beforeDestroy: function() {
    if (this.pdfdata) this.pdfdata.destroy();
    this.pdfdata = null;
  }
};
</script>