<template>
  <div class="container">
    <div class="calculator">
      <div class="calculator__screen">
        {{ display }}
      </div>
      <div @click="reset()" class="calculator__btn light-gray">AC</div>
      <div @click="plusMinus()" class="calculator__btn light-gray">+/-</div>
      <div @click="calcPercentage('%')" class="calculator__btn light-gray">%</div>
      <div @click="applyOperation('/')" class="calculator__btn orange">÷</div>
      <div @click="handleButtonClick('7')" class="calculator__btn">7</div>
      <div @click="handleButtonClick('8')" class="calculator__btn">8</div>
      <div @click="handleButtonClick('9')" class="calculator__btn">9</div>
      <div @click="applyOperation('*')" class="calculator__btn orange">X</div>
      <div @click="handleButtonClick('4')" class="calculator__btn">4</div>
      <div @click="handleButtonClick('5')" class="calculator__btn">5</div>
      <div @click="handleButtonClick('6')" class="calculator__btn">6</div>
      <div @click="applyOperation('+')" class="calculator__btn orange">+</div>
      <div @click="handleButtonClick('1')" class="calculator__btn">1</div>
      <div @click="handleButtonClick('2')" class="calculator__btn">2</div>
      <div @click="handleButtonClick('3')" class="calculator__btn">3</div>
      <div @click="applyOperation('-')" class="calculator__btn orange">-</div>
      <div @click="handleButtonClick('0')" class="calculator__btn zero">0</div>
      <div @click="handleButtonClick('.')" class="calculator__btn">,</div>
      <div @click="equal()" class="calculator__btn">=</div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import { evaluate } from 'mathjs'

export default {
  setup() {
    const display = ref('0')
    const memory = ref(null)
    const sign = ref(null)
    const lastSign = ref(null)
    let result = '0'

    function handleButtonClick(value) {
      if (sign.value) {
        display.value = '0'
        sign.value = ''
      }
      if (value === '.' && display.value === '0') {
        display.value = '0' + value
      }

      if (display.value[0] === '0' && display.value[1] !== '.') {
        display.value = display.value.slice(1)
      }

      if (value === '.' && display.value.includes('.')) {
        return
      }
      display.value += value
    }

    function applyOperation(operator) {
      sign.value = operator
      lastSign.value = operator
      calculation()
    }

    function calculation() {
      if (!memory.value) {
        memory.value = display.value

        return
      } else {
        result = evaluate(`${memory.value}${sign.value}${display.value}`)
        memory.value = result
        display.value = result
      }
    }

    function equal() {
      sign.value = '='
      if (lastSign.value && lastSign.value !== '=') {
        result = evaluate(`${memory.value}${lastSign.value}${display.value}`)
        memory.value = null
        display.value = result
      }
    }

    function reset() {
      result = 0
      display.value = '0'
      memory.value = null
      sign.value = null
      lastSign.value = null
    }
    function calcPercentage(operator) {
      sign.value = operator
      lastSign.value = operator
      display.value = display.value / 100
    }

    function plusMinus() {
      if (+display.value < 0) {
        display.value *= -1
      } else {
        display.value = -display.value
      }
    }

    return {
      display,
      handleButtonClick,
      equal,
      applyOperation,
      reset,
      calcPercentage,
      plusMinus
    }
  }
}
</script>

<style scoped></style>
