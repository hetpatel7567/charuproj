<template>
  <div>
    <div class="controls">
      <button @click="prevPage">Previous</button>
      <button @click="nextPage">Next</button>
      <span>Page {{ pageNumber }} of {{ totalPages }}</span>
    </div>
    <canvas ref="pdfCanvas"></canvas>
  </div>
</template>

<script>
import { getDocument, GlobalWorkerOptions } from "pdfjs-dist/webpack"; // Correct import path

export default {
  data() {
    return {
      pdf: null,
      pageNumber: 1,
      totalPages: 0,
    };
  },
  mounted() {
    // Configure the worker source
    GlobalWorkerOptions.workerSrc = `https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.worker.min.js`;
    this.loadPdf("/24DCE091.pdf"); // Update with the actual PDF path
  },
  methods: {
    async loadPdf(url) {
      const loadingTask = getDocument(url);
      this.pdf = await loadingTask.promise;
      this.totalPages = this.pdf.numPages;
      this.renderPage(this.pageNumber);
    },
    async renderPage(pageNum) {
      const page = await this.pdf.getPage(pageNum);
      const viewport = page.getViewport({ scale: 1.5 });
      const canvas = this.$refs.pdfCanvas;
      const context = canvas.getContext("2d");

      canvas.width = viewport.width;
      canvas.height = viewport.height;

      const renderContext = {
        canvasContext: context,
        viewport: viewport,
      };
      await page.render(renderContext).promise;
    },
    nextPage() {
      if (this.pageNumber < this.totalPages) {
        this.pageNumber++;
        this.renderPage(this.pageNumber);
      }
    },
    prevPage() {
      if (this.pageNumber > 1) {
        this.pageNumber--;
        this.renderPage(this.pageNumber);
      }
    },
  },
};
</script>

<style scoped>
.controls {
  margin-bottom: 10px;
}
canvas {
  border: 1px solid #ccc;
  display: block;
  margin: 0 auto;
}
</style>
