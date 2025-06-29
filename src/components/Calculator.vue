<script setup>
import { ref, onMounted } from 'vue';
import { evaluate } from 'mathjs';
import Display from './Display.vue';
import Keypad from './Keypad.vue';

defineProps({ title: String });

const expression = ref('');
const result = ref('');
const justCalculated = ref(false);

const append = (val) => {
  const isOperator = /[+\-*/]/.test(val);
  const isNumber = /[0-9.]/.test(val);

  if (result.value === 'Error' || result.value === 'NaN') {
    expression.value = isNumber ? val : '';
    result.value = '';
    justCalculated.value = false;
    return;
  }

  if (justCalculated.value) {
    if (isOperator) {
      expression.value = result.value.toString() + val;
    } else if (isNumber) {
      expression.value = val;
      result.value = '';
    }
    justCalculated.value = false;
    return;
  }

  const lastChar = expression.value.slice(-1);

  if (/[+\-*/.]/.test(val) && /[+\-*/.]/.test(lastChar)) return;

  if (val === ')') {
    const open = (expression.value.match(/\(/g) || []).length;
    const close = (expression.value.match(/\)/g) || []).length;
    if (close >= open) return;
  }

  if (expression.value === '' && /[*/.)]/.test(val)) return;

  expression.value += val;
}

const clear = () => {
  expression.value = '';
  result.value = '';
}

const deleteLast = () => {
  expression.value = expression.value.slice(0, -1);
}

const calculate = () => {
  try {
    const evalResult = evaluate(expression.value);
    result.value = evalResult.toString();
    justCalculated.value = true;
  } catch (err) {
    result.value = 'Error';
    justCalculated.value = false;
  }
}

const square = () => {
  if (expression.value === '') return;

  const lastChar = expression.value.slice(-1);

  if (!/[0-9)]/.test(lastChar)) return;

  expression.value = `(${expression.value})^2`;
}

const negate = () => {
  if (expression.value === '') return;

  const lastChar = expression.value.slice(-1);

  if (!/[0-9)]/.test(lastChar)) return;

  expression.value = `-(${expression.value})`;
}

const handleKey = (e) => {
  if (e.ctrlKey || e.metaKey) return

  if ('0123456789+-*/().^'.includes(e.key)) {
    e.preventDefault();
    append(e.key)
  } else if (e.key === 'Enter' || e.key === '=') {
    e.preventDefault();
    calculate();
  } else if (e.key === 'Backspace') {
    e.preventDefault();
    deleteLast();
  } else if (e.key.toLowerCase() === 'c' || e.key === 'Escape') {
    e.preventDefault();
    clear();
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKey);
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
        @square="square"
        @negate="negate"
      />
    </div>
  </div>
</template>