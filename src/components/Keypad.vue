<script setup>
const emit = defineEmits(['append', 'clear', 'delete', 'calculate', 'square', 'negate'])
;

const keys = [
  ['7', '8', '9', '/'],
  ['4', '5', '6', '*'],
  ['1', '2', '3', '-'],
  ['0', '.', '=', '+'],
];

const handleClick = (key) => {
  if (key === '=') {
    emit('calculate')
  } else {
    emit('append', key)
  }
};
</script>

<template>
  <div>
    <div class="grid grid-cols-4 gap-2 mb-2">
      <button
        @click="emit('append', '(')"
        class="bg-gray-200 py-2 rounded text-lg"
      >
        (
      </button>
      <button
        @click="emit('append', ')')"
        class="bg-gray-200 py-2 rounded text-lg"
      >
        )
      </button>
      <button
        @click="emit('clear')"
        class="bg-red-500 text-white py-2 rounded text-lg"
      >
        C
      </button>
      <button
        @click="emit('delete')"
        class="bg-yellow-500 text-white py-2 rounded text-lg"
      >
        ⌫
      </button>
      
      <button
            @click="emit('append', 'sqrt(')"
            class="bg-gray-200 py-2 rounded text-lg"
        >
            √
        </button>
        <button
            @click="emit('square')"
            class="bg-gray-200 py-2 rounded text-lg"
        >
            x²
        </button>
        <button
            @click="emit('append', '^')"
            class="bg-gray-200 py-2 rounded text-lg"
        >
            ^
        </button>
        <button
            @click="emit('negate')"
            class="bg-gray-200 py-2 rounded text-lg"
        >
            ±
        </button>
    </div>

    <div class="grid grid-cols-4 gap-2">
      <template v-for="row in keys" :key="row">
        <button
          v-for="key in row"
          :key="key"
          @click="handleClick(key)"
          class="bg-gray-100 hover:bg-gray-300 py-2 rounded text-lg"
        >
          {{ key }}
        </button>
      </template>
    </div>
  </div>
</template>