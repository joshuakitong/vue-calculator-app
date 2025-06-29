<script setup>
import { ref, onMounted } from 'vue'
import Display from './Display.vue'
import Keypad from './Keypad.vue'

defineProps({ title: String })

const expression = ref('')
const result = ref('')

const append = (val) => {
  expression.value += val
}

const clear = () => {
  expression.value = ''
  result.value = ''
}

const deleteLast = () => {
  expression.value = expression.value.slice(0, -1)
}

const calculate = () => {
  try {
    result.value = eval(expression.value)
  } catch {
    result.value = 'Error'
  }
}

// Keyboard input
const handleKey = (e) => {
  if ('0123456789+-*/().'.includes(e.key)) {
    append(e.key)
  } else if (e.key === 'Enter') {
    calculate()
  } else if (e.key === 'Backspace') {
    deleteLast()
  } else if (e.key.toLowerCase() === 'c') {
    clear()
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKey)
})
</script>

<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100 px-4">
    <div class="w-full max-w-sm bg-white rounded-xl shadow-xl p-4">
      <h1 class="text-center text-2xl font-bold mb-4">{{ title }}</h1>
      <Display :expression="expression" :result="result" />
      <Keypad
        @append="append"
        @clear="clear"
        @delete="deleteLast"
        @calculate="calculate"
      />
    </div>
  </div>
</template>