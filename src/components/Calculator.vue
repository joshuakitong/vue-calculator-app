<script setup>
import { ref } from 'vue'

defineProps({
  title: String,
})

</script>

<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100 text-gray-700 text-3xl font-bold">
    <h1>{{ title }}</h1>
  </div>
</template>
