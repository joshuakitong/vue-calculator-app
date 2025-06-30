<script setup>
import { ref, onMounted } from 'vue';
import { evaluate } from 'mathjs';
import Display from './Display.vue';
import Keypad from './Keypad.vue';

defineProps({ title: String });

const expression = ref('');
const result = ref('');
const justCalculated = ref(false);
const operators = /[+\-*/^.]/;

const append = (val) => {
  const isOperator = operators.test(val);
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
    } else if (val === 'sqrt(') {
      expression.value = `sqrt(${result.value})`;
    } else if (val === '(') {
      expression.value = result.value.toString() + val;
    }
    justCalculated.value = false;
    return;
  }

  if (justCalculated.value && val === 'sqrt(') {
    expression.value = `sqrt(${result.value})`;
    justCalculated.value = false;
    return;
  }

  const lastChar = expression.value.slice(-1);

  if (val === '.') {
    const lastOperatorIndex = Math.max(
      expression.value.lastIndexOf('+'),
      expression.value.lastIndexOf('-'),
      expression.value.lastIndexOf('*'),
      expression.value.lastIndexOf('/'),
      expression.value.lastIndexOf('(')
    );

    const currentNumber = expression.value.slice(lastOperatorIndex + 1)
    if (currentNumber.includes('.')) return;
  }

  const isCurrentOp = operators.test(val)
  const isLastOp = operators.test(lastChar)
  const allowNegativeExponent = lastChar === '^' && val === '-'

  if (isCurrentOp && isLastOp && !allowNegativeExponent) {
    if (val === '.') {
      const lastOperatorIndex = Math.max(
        expression.value.lastIndexOf('+'),
        expression.value.lastIndexOf('-'),
        expression.value.lastIndexOf('*'),
        expression.value.lastIndexOf('/'),
        expression.value.lastIndexOf('('),
        expression.value.lastIndexOf('^')
      )
      const currentNumber = expression.value.slice(lastOperatorIndex + 1)
      if (currentNumber.includes('.')) return
    }

    expression.value = expression.value.slice(0, -1) + val
    return
  }

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
  justCalculated.value = false;
}

const deleteLast = () => {
  expression.value = expression.value.slice(0, -1);
}

const calculate = () => {
  try {
    const open = (expression.value.match(/\(/g) || []).length;
    const close = (expression.value.match(/\)/g) || []).length;
    let balancedExpression = expression.value + ')'.repeat(open - close);

    let evalResult = evaluate(balancedExpression);

    result.value = evalResult.toString();
    justCalculated.value = true;
  } catch (err) {
    result.value = 'Error';
    justCalculated.value = false;
  }
};

const square = () => {
  if (expression.value === '') return;

  const lastChar = expression.value.slice(-1);

  if (!/[0-9)]/.test(lastChar)) return;

  const valueToSquare = justCalculated.value ? result.value : expression.value;

  expression.value = `((${valueToSquare})^2)`;
  justCalculated.value = false;
}

const negate = () => {
  if (expression.value === '') return;

  const lastChar = expression.value.slice(-1);

  if (!/[0-9)]/.test(lastChar)) return;

  const valueToNegate = justCalculated.value ? result.value : expression.value;
  if (!valueToNegate) return;

  expression.value = `-(${valueToNegate})`;
  justCalculated.value = false;
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
  <div class="min-h-screen flex items-center justify-center bg-gray-200 px-4">
    <div class="w-full max-w-md bg-gray-600 rounded-xl shadow-xl p-4">
      <h1 class="text-center text-2xl font-bold mb-4 text-white">{{ title }}</h1>
      <Display :expression="expression" :result="result" :justCalculated="justCalculated" />
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