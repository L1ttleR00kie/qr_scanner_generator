<template>
  <label class="file-select">
    <div class="select-button">
      <span v-if="file">Selected File: {{file.name}}</span>
      <span v-else>Select File</span>
    </div>
    <input type="file" @change="handleFileChange"/>
  </label>
</template>

<script setup>
import { ref, defineEmits } from 'vue';

const emits = defineEmits(['update:modelValue']);

const handleFileChange = (e) => {
  const file = e.target.files[0];

  const allowedTypes = ['application/vnd.openxmlformats-officedocument.spreadsheetml.sheet', 'text/csv', 'application/json'];

  if (!allowedTypes.includes(file.type)) {
      console.log('Selected file is not allowed.')
  }

  emits('update:modelValue', file);

};

const file = ref(null);
</script>