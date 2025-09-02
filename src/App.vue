<script setup lang="ts">
import { ref } from 'vue'

const display = ref('0')
const previousValue = ref(0)
const operation = ref('')
const waitingForValue = ref(false)

const inputNumber = (num: string) => {
  if (waitingForValue.value) {
    display.value = num
    waitingForValue.value = false
  } else {
    display.value = display.value === '0' ? num : display.value + num
  }
}

const inputOperation = (nextOperation: string) => {
  const inputValue = parseFloat(display.value)

  if (previousValue.value === 0) {
    previousValue.value = inputValue
  } else if (operation.value) {
    const currentValue = previousValue.value || 0
    const newValue = calculate(currentValue, inputValue, operation.value)

    display.value = String(newValue)
    previousValue.value = newValue
  }

  waitingForValue.value = true
  operation.value = nextOperation
}

const calculate = (firstValue: number, secondValue: number, op: string): number => {
  switch (op) {
    case '+':
      return firstValue + secondValue
    case '-':
      return firstValue - secondValue
    case '×':
      return firstValue * secondValue
    case '÷':
      return secondValue !== 0 ? firstValue / secondValue : 0
    default:
      return secondValue
  }
}

const performCalculation = () => {
  const inputValue = parseFloat(display.value)

  if (previousValue.value && operation.value) {
    const newValue = calculate(previousValue.value, inputValue, operation.value)
    display.value = String(newValue)
    previousValue.value = 0
    operation.value = ''
    waitingForValue.value = true
  }
}

const clearAll = () => {
  display.value = '0'
  previousValue.value = 0
  operation.value = ''
  waitingForValue.value = false
}

const clearEntry = () => {
  display.value = '0'
}

const toggleSign = () => {
  if (display.value !== '0') {
    display.value = display.value.charAt(0) === '-' 
      ? display.value.slice(1) 
      : '-' + display.value
  }
}

const inputDot = () => {
  if (waitingForValue.value) {
    display.value = '0.'
    waitingForValue.value = false
  } else if (display.value.indexOf('.') === -1) {
    display.value += '.'
  }
}
</script>

<template>
  <div class="calculator">
    <div class="display">
      {{ display }}
    </div>
    
    <div class="buttons">
      <button @click="clearAll" class="btn function">AC</button>
      <button @click="clearEntry" class="btn function">CE</button>
      <button @click="toggleSign" class="btn function">±</button>
      <button @click="inputOperation('÷')" class="btn operation">÷</button>
      
      <button @click="inputNumber('7')" class="btn number">7</button>
      <button @click="inputNumber('8')" class="btn number">8</button>
      <button @click="inputNumber('9')" class="btn number">9</button>
      <button @click="inputOperation('×')" class="btn operation">×</button>
      
      <button @click="inputNumber('4')" class="btn number">4</button>
      <button @click="inputNumber('5')" class="btn number">5</button>
      <button @click="inputNumber('6')" class="btn number">6</button>
      <button @click="inputOperation('-')" class="btn operation">−</button>
      
      <button @click="inputNumber('1')" class="btn number">1</button>
      <button @click="inputNumber('2')" class="btn number">2</button>
      <button @click="inputNumber('3')" class="btn number">3</button>
      <button @click="inputOperation('+')" class="btn operation">+</button>
      
      <div class="zero-container">
        <button @click="inputNumber('0')" class="btn number zero-btn">0</button>
      </div>
      <button @click="inputDot" class="btn number">.</button>
      <button @click="performCalculation" class="btn equals">=</button>
    </div>
  </div>
</template>

<style scoped>
body, html {
  margin: 0;
  padding: 0;
  height: 100vh;
}

.calculator {
  width: 300px;
  max-width: 320px;
  margin: 0 auto;
  background: #333;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

@media (max-width: 480px) {
  .calculator {
    width: 280px;
    max-width: 85vw;
    padding: 15px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  
  .display {
    font-size: 1.8rem;
    padding: 12px;
    margin-bottom: 12px;
  }
  
  .btn {
    height: 45px;
    font-size: 0.9rem;
  }
  
  .buttons {
    gap: 6px;
  }
}

.display {
  background: #000;
  color: #fff;
  font-size: 2.2rem;
  padding: 16px;
  text-align: right;
  margin-bottom: 16px;
  border-radius: 5px;
  min-height: 45px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  font-family: 'Courier New', monospace;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 8px;
}

.btn {
  height: 65px;
  border: none;
  border-radius: 5px;
  font-size: 1.1rem;
  cursor: pointer;
  transition: all 0.2s ease;
  aspect-ratio: 1;
}

.btn:hover {
  transform: scale(0.95);
}

.btn:active {
  transform: scale(0.9);
}

.number {
  background: #505050;
  color: #fff;
}

.number:hover {
  background: #606060;
}

.operation {
  background: #ff9500;
  color: #fff;
}

.operation:hover {
  background: #ffad33;
}

.function {
  background: #a6a6a6;
  color: #000;
}

.function:hover {
  background: #bfbfbf;
}

.equals {
  background: #ff9500;
  color: #fff;
}

.equals:hover {
  background: #ffad33;
}



.zero-container {
  grid-column: span 2;
  display: flex;
}

.zero-btn {
  width: 100%;
  margin: 0;
}

</style>
