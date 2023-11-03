<template>
  <div class="container">
    <div class="calculator">
      <div class="display">{{ current || '0' }}</div>
      <div @click="clear" class="btn">C</div>
      <div @click="sign" class="btn">+/-</div>
      <div @click="percent" class="btn">%</div>
      <div @click="performOperation('divide')" class="btn operator">÷</div>
      <div v-for="num in [7, 8, 9]" :key="num" @click="append(num)" class="btn">{{ num }}</div>
      <div @click="performOperation('times')" class="btn operator">x</div>
      <div v-for="num in [4, 5, 6]" :key="num" @click="append(num)" class="btn">{{ num }}</div>
      <div @click="performOperation('minus')" class="btn operator">-</div>
      <div v-for="num in [1, 2, 3]" :key="num" @click="append(num)" class="btn">{{ num }}</div>
      <div @click="performOperation('plus')" class="btn operator">+</div>
      <div @click="append('0')" class="btn zero">0</div>
      <div @click="dot" class="btn">.</div>
      <div @click="equal" class="btn operator">=</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      previous: null,
      current: '',
      operator: null,
      operatorClicked: false,
    };
  },
  methods: {
    clear() {
      this.current = '';
    },
    sign() {
      this.current = this.current.charAt(0) === '-' ? this.current.slice(1) : `-${this.current}`;
    },
    percent() {
      this.current = `${parseFloat(this.current) / 100}`;
    },
    append(value) {
      if (this.operatorClicked) {
        this.current = '';
        this.operatorClicked = false;
      }
      this.current = `${this.current}${value}`;
    },
    dot() {
      if (this.current.indexOf('.') === -1) {
        this.append('.');
      }
    },
    setPrevious() {
      this.previous = this.current;
      this.operatorClicked = true;
    },
    performOperation(op) {
      this.operator = op;
      this.setPrevious();
    },
    equal() {
      const a = parseFloat(this.current);
      const b = parseFloat(this.previous);
      switch (this.operator) {
        case 'plus':
          this.current = `${a + b}`;
          break;
        case 'minus':
          this.current = `${b - a}`;
          break;
        case 'times':
          this.current = `${a * b}`;
          break;
        case 'divide':
          this.current = b !== 0 ? `${b / a}` : 'Error';
          break;
      }
      this.previous = null;
    },
  },
};
</script>

<style scoped>
.calculator {
  margin: 0 auto;
  display: grid;
  width: 250px;
  font-size: 40px;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}

.display {
  display: flex;
  justify-content: center;
  align-items: center;
  grid-column: 1/5;
  padding: 16px;
  color: #fff;
  background-color: rgb(0, 31, 55);
}

.zero {
  grid-column: 1/3;
}

.btn {
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 0;
  background-color: #fff;
  border: 1px solid #999;
}

.operator {
  background-color: orange;
  color: #fff;
}
</style>
