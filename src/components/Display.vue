<script setup>
defineProps({
  expression: String,
  result: String,
})
</script>

<template>
  <div class="bg-gray-900 text-white p-4 h-28 rounded text-right space-y-2 mb-4">
    <div class="text-lg text-gray-400 truncate">{{ expression || '0' }}</div>
    <div class="text-4xl font-mono">{{ result || '' }}</div>
  </div>
</template>