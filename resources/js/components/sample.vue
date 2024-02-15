<template>
    <div>
       {{ props.foo }}
    </div>
</template>

<script setup>
const props = defineProps({
  foo: String
})
</script>