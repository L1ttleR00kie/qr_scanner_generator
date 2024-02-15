<template>
  <div class="dropdown">
    <button @click="toggleDropdown">Download</button>
    <div v-if="showDropdown" class="dropdown-content">
      <a :href="downloadUrl" @click="downloadZip">Download as zip file</a>
      <a :href="downloadUrl" @click="downloadZip">Download as zip file</a>
      <a :href="downloadUrl" @click="downloadZip">Download as zip file</a>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import JSZip from 'jszip';

// Define a prop to receive qrDataUrls
const props = defineProps({
  qrDataUrls: Array,
  file: Object,
});

let downloadUrl = '';
const showDropdown = ref(false);

const toggleDropdown = () => {
  showDropdown.value = !showDropdown.value;
};

const downloadZip = async () => {
  try {
    const zip = new JSZip();

    // Add each QR code as a file to the zip
    props.qrDataUrls.forEach((qrDataURL, index) => {
      zip.file(`qrcode_${props.qrGetName}${index}.png`, qrDataURL.split(',')[1], { base64: true });
    });

    // Generate the zip file
    const content = await zip.generateAsync({ type: 'blob' });

    // Create a temporary link to download the zip file
    const a = document.createElement('a');
    a.href = URL.createObjectURL(content);
    a.download = `qr_codes_${props.file.name}.zip`;
    a.click();
  } catch (error) {
    console.error('Error creating zip file:', error);
  }
};
</script>

<style scoped>
.dropdown {
  position: relative;
  display: inline-block;
}

.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f9f9f9;
  min-width: 160px;
  box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
  z-index: 1;
}

.dropdown-content a {
  color: black;
  padding: 12px 10px;
  text-decoration: none;
  display: block;
}

.dropdown-content a:hover {
  background-color: #f1f1f1;
}

.dropdown:hover .dropdown-content {
  display: block;
}

.dropdown:hover .dropbtn {
  background-color: #3e8e41;
}
</style>