<script setup>
defineProps({
  expression: String,
  result: String,
});
</script>

<template>
  <div class="bg-gray-800 text-white p-4 rounded mb-4 h-26 flex flex-col text-right">
    <div class="text-lg text-gray-400 truncate">
      {{ expression || '0' }}
    </div>
    <div class="text-3xl font-mono mt-auto">
      {{ result || '' }}
    </div>
  </div>
</template>