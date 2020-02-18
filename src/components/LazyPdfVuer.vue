<template>
  <div v-if="pdfdata">
    <template v-for="i in numPages">
      <template v-if="i < 10">
        <pdfvuer :src="pdfdata" :page="i" :key="'page' + i" :id="i"></pdfvuer>
      </template>
      <template v-else>
        <div :key="'page' + i" :id="i">haha</div>
      </template>
    </template>
  </div>
</template>
 
<script>
import pdfvuer from "pdfvuer";

export default {
  components: {
    pdfvuer
  },
  props: {
    path: String
  },
  data: function() {
    return {
      pdfdata: undefined,
      page: 0,
      currentPage: 1,
      numPages: 0
    };
  },
  mounted: function() {
    this.pdfdata = pdfvuer.createLoadingTask(this.path);
    this.pdfdata.then(pdf => {
      this.numPages = pdf.numPages;

      window.onscroll = function() {
        changePage();
      };

      let self = this;

      function findPos(obj) {
        return obj.offsetTop;
      }

      function changePage() {
        var i = 1,
          count = Number(pdf.numPages);

        do {
          if (
            window.pageYOffset >= findPos(document.getElementById(i)) &&
            window.pageYOffset <= findPos(document.getElementById(i + 1))
          ) {
            self.page = i;
          }
          i++;
        } while (i < count);

        if (window.pageYOffset >= findPos(document.getElementById(i))) {
          self.page = i;
        }

        console.log(self.page);
      }
    });
  }
};
</script>