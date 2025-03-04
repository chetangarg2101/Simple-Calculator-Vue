<script setup>
import { ref, onMounted } from "vue";

const display = ref("");
const lastResult = ref(null);
const lastInputWasOperator = ref(false);

const appendValue = (value) => {
  if (lastResult.value !== null && !lastInputWasOperator.value) {
    display.value = "";
    lastResult.value = null;
  }
  display.value += value;
  lastInputWasOperator.value = false;
};

const clearDisplay = () => {
  display.value = "";
  lastResult.value = null;
};

const backspace = () => {
  display.value = display.value.slice(0, -1);
};

const calculateResult = () => {
  try {
    display.value = eval(display.value).toString();
    lastResult.value = display.value;
  } catch (error) {
    display.value = "Error";
  }
};

const calculateSquare = () => {
  try {
    display.value = Math.pow(eval(display.value), 2).toString();
    lastResult.value = display.value;
  } catch (error) {
    display.value = "Error";
  }
};

const calculateSquareRoot = () => {
  try {
    display.value = Math.sqrt(eval(display.value)).toString();
    lastResult.value = display.value;
  } catch (error) {
    display.value = "Error";
  }
};

const calculatePercentage = () => {
  try {
    display.value = (eval(display.value) / 100).toString();
    lastResult.value = display.value;
  } catch (error) {
    display.value = "Error";
  }
};

const appendOperator = (operator) => {
  if (lastResult.value !== null) {
    display.value = lastResult.value;
  }
  if ("+-*/".includes(display.value.slice(-1))) {
    display.value = display.value.slice(0, -1);
  }
  display.value += operator;
  lastInputWasOperator.value = true;
};

// Handle keyboard input
const handleKeyPress = (event) => {
  const key = event.key;
  if (!isNaN(key) || key === ".") {
    appendValue(key);
  } else if ("+-*/".includes(key)) {
    appendOperator(key);
  } else if (key === "Enter") {
    calculateResult();
  } else if (key === "Backspace") {
    backspace();
  } else if (key === "Escape") {
    clearDisplay();
  }
};

// Attach keyboard event listener
onMounted(() => {
  window.addEventListener("keydown", handleKeyPress);
});
</script>

<template>
  <div class="container">
    <div class="calculator">
      <h2 class="title">Calculator</h2>
      <input v-model="display" disabled class="display" />
      <div class="buttons">
        <button @click="clearDisplay" class="operator">C</button>
        <button @click="backspace" class="operator">⌫</button>
        <button @click="calculatePercentage" class="operator">%</button>
        <button @click="appendOperator('/')" class="operator">÷</button>

        <button @click="appendValue('7')">7</button>
        <button @click="appendValue('8')">8</button>
        <button @click="appendValue('9')">9</button>
        <button @click="appendOperator('*')" class="operator">×</button>

        <button @click="appendValue('4')">4</button>
        <button @click="appendValue('5')">5</button>
        <button @click="appendValue('6')">6</button>
        <button @click="appendOperator('-')" class="operator">−</button>

        <button @click="appendValue('1')">1</button>
        <button @click="appendValue('2')">2</button>
        <button @click="appendValue('3')">3</button>
        <button @click="appendOperator('+')" class="operator">+</button>

        <button @click="appendValue('0')">0</button>
        <button @click="appendValue('.')">.</button>
        <button @click="calculateSquareRoot" class="operator">√x</button>
        <button @click="calculateSquare" class="operator">x²</button>

        <button @click="calculateResult" class="equal">=</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Background */
.container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #1e3c72, #ffffff);
}

/* Calculator */
.calculator {
  width: 320px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(15px);
  border-radius: 15px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
  text-align: center;
}

/* Title */
.title {
  font-size: 24px;
  font-weight: bold;
  color: white;
  margin-bottom: 10px;
}

/* Display */
.display {
  width: 100%;
  height: 60px;
  font-size: 28px;
  text-align: right;
  margin-bottom: 15px;
  padding: 10px;
  border: none;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.1);
  color: white;
  outline: none;
  font-weight: bold;
}

/* Buttons */
.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}

button {
  width: 100%;
  height: 50px;
  font-size: 20px;
  font-weight: bold;
  border: none;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.2);
  color: white;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background: rgba(255, 255, 255, 0.4);
}

.operator {
  background: rgba(255, 87, 51, 0.8);
}

.operator:hover {
  background: rgba(255, 87, 51, 1);
}

.equal {
  background: rgba(76, 175, 80, 0.8);
  grid-column: span 4;
}

.equal:hover {
  background: rgba(76, 175, 80, 1);
}
</style>
