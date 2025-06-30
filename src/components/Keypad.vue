<script setup>
const emit = defineEmits(['append', 'clear', 'delete', 'calculate', 'square', 'negate']);

const keys = [
  [
    { label: '(', class: 'bg-gray-300 hover:bg-gray-400', action: () => emit('append', '(') },
    { label: ')', class: 'bg-gray-300 hover:bg-gray-400', action: () => emit('append', ')') },
    { label: 'C', class: 'bg-red-400 hover:bg-red-500 text-white', action: () => emit('clear') },
    { label: '⌫', class: 'bg-yellow-400 hover:bg-yellow-500 text-white', action: () => emit('delete') },
  ],
  [
    { label: '√', class: 'bg-gray-300 hover:bg-gray-400', action: () => emit('append', 'sqrt(') },
    { label: 'x²', class: 'bg-gray-300 hover:bg-gray-400', action: () => emit('square') },
    { label: '^', class: 'bg-gray-300 hover:bg-gray-400', action: () => emit('append', '^') },
    { label: '/', class: 'bg-gray-300 hover:bg-gray-400', action: () => emit('append', '/') },
  ],
  [
    { label: '7', class: 'bg-gray-100 hover:bg-gray-200', action: () => emit('append', '7') },
    { label: '8', class: 'bg-gray-100 hover:bg-gray-200', action: () => emit('append', '8') },
    { label: '9', class: 'bg-gray-100 hover:bg-gray-200', action: () => emit('append', '9') },
    { label: '*', class: 'bg-gray-300 hover:bg-gray-400', action: () => emit('append', '*') },
  ],
  [
    { label: '4', class: 'bg-gray-100 hover:bg-gray-200', action: () => emit('append', '4') },
    { label: '5', class: 'bg-gray-100 hover:bg-gray-200', action: () => emit('append', '5') },
    { label: '6', class: 'bg-gray-100 hover:bg-gray-200', action: () => emit('append', '6') },
    { label: '-', class: 'bg-gray-300 hover:bg-gray-400', action: () => emit('append', '-') },
  ],
  [
    { label: '1', class: 'bg-gray-100 hover:bg-gray-200', action: () => emit('append', '1') },
    { label: '2', class: 'bg-gray-100 hover:bg-gray-200', action: () => emit('append', '2') },
    { label: '3', class: 'bg-gray-100 hover:bg-gray-200', action: () => emit('append', '3') },
    { label: '+', class: 'bg-gray-300 hover:bg-gray-400', action: () => emit('append', '+') },
  ],
  [
    { label: '±', class: 'bg-gray-100 hover:bg-gray-200', action: () => emit('negate') },
    { label: '0', class: 'bg-gray-100 hover:bg-gray-200', action: () => emit('append', '0') },
    { label: '.', class: 'bg-gray-100 hover:bg-gray-200', action: () => emit('append', '.') },
    { label: '=', class: 'bg-blue-400 hover:bg-blue-500', action: () => emit('calculate') },
  ],
];

</script>

<template>
    <div class="grid grid-cols-4 gap-2" v-for="(row, rowIndex) in keys" :key="rowIndex">
        <button
            v-for="(key, keyIndex) in row"
            :key="keyIndex"
            @click="key.action"
            :class="['py-2 rounded text-lg my-1 cursor-pointer', key.class]"
        >
            {{ key.label }}
        </button>
    </div>
</template>