<script setup>
import { onMounted } from "vue";

// Function to dynamically load Adobe Embed API script
function loadAdobeScript() {
  const script = document.createElement("script");
  script.src = "https://documentcloud.adobe.com/view-sdk/main.js";
  script.onload = initializeAdobeViewer; // Initialize viewer after the script loads
  document.body.appendChild(script);
}

// Function to initialize the Adobe PDF Viewer
function initializeAdobeViewer() {
  if (!window.AdobeDC) {
    console.error("AdobeDC is not defined. The script might not have loaded.");
    return;
  }

  // Adobe Viewer instance
  const adobeDCView = new AdobeDC.View({
    clientId: "2bf275678c154f69aedf46f946c20170", // Replace with your actual API Key
    divId: "pdf-div",
  });

  // Preview the PDF
  adobeDCView.previewFile(
    {
      content: {
        location: {
          url: "/public/24DCE091.pdf", // Ensure this path is correct relative to your `public` folder
        },
      },
      metaData: {
        fileName: "24DCE091.pdf",
      },
    },
    {
      embedMode: "SIZED_CONTAINER", // Options: "SIZED_CONTAINER", "FULL_WINDOW", "IN_LINE"
    }
  );
}

// Call loadAdobeScript on component mount
onMounted(() => {
  loadAdobeScript();
});
</script>

<template>
  <div>
    <h1>Adobe PDF Viewer</h1>
    <!-- Div where the PDF will be displayed -->
    <div id="pdf-div" style="width: 100%; height: 600px; border: 1px solid #ccc;"></div>
  </div>
</template>

<style scoped>
h1 {
  color: white;
  font-family: Arial, sans-serif;
  text-align: center;
  margin-bottom: 20px;
}
</style>
