<script setup>
defineProps({
  expression: String,
  result: String,
});
</script>

<template>
  <div class="bg-gray-800 text-white p-4 h-26 rounded text-right space-y-2 mb-4">
    <div class="text-lg text-gray-400 truncate">{{ expression || '0' }}</div>
    <div class="text-3xl font-mono">{{ result || '' }}</div>
  </div>
</template>