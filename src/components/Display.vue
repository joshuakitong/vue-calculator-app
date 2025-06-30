<script setup>
defineProps({
  expression: String,
  result: String,
  justCalculated: Boolean,
});
</script>

<template>
  <div class="bg-gray-800 text-white p-4 rounded mb-4 h-26 flex flex-col text-right">
    

    <div class="text-lg text-gray-400 truncate">
      {{ justCalculated ? expression : '' }}
    </div>

    <div class="text-3xl font-mono mt-auto truncate">
      {{ justCalculated ? result : expression || '0' }}
    </div>

  </div>
</template>