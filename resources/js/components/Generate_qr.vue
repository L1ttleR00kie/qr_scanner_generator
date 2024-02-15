<template>
  <div>
    <select_file v-model="file" @update:modelValue="handleFileChange"/>
    <download_zip :qrDataUrls="qrDataUrls" :file="file" :qrGetName="qrGetName"/>
    <div v-if="qrDataUrls.length > 0" class="row-container">
      <div v-for="(qrDataURL, index) in qrDataUrls" :key="index">
        <img :src="qrDataURL" alt="QR Code" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import select_file from './select_file.vue'
import download_zip from './download_zip.vue'
import QRCode from 'qrcode'
import * as XLSX from 'xlsx'



const file = ref(null)
const qrDataUrls = ref([])
const qrGetName = ref([])

const handleFileChange = async (file) => {
  if (!file) {
    console.error('No file selected.')
    return
  }
  
  const fileReader = new FileReader()

  fileReader.onload = (e) => {
    const data = new Uint8Array(e.target.result);
    const workbook = XLSX.read(data, { type: 'array' });
    const sheetName = workbook.SheetNames[0];
    const worksheet = workbook.Sheets[sheetName];
    const jsonData = XLSX.utils.sheet_to_json(worksheet, { header: 1 });
    generateQRCodeForEachEntry(jsonData.slice(1));
  };
  
  fileReader.readAsArrayBuffer(file)
}

const generateQRCodeForEachEntry = async (dataEntries) => {
  try {
    const qrDataUrlsArray = await Promise.all(dataEntries.map(async (row) => {
      const contentString = JSON.stringify(row[1]);
      return await QRCode.toDataURL(contentString);
    }));
    qrDataUrls.value = qrDataUrlsArray;

    const qrDataNameArray = await Promise.all(dataEntries.map(async (row) => {
      return JSON.stringify(row[0]);
    }));
    qrGetName.value = qrDataNameArray;

  } catch (error) {
    console.error('Error generating QR codes:', error);
  }
}
</script>

<style>
.row-container {
  display: flex;
  flex-wrap: wrap;
}
</style>
