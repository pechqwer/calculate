<template >
  <div class="calculate" @keypress.prevent="addKeyCode">
    <div class="history" @click="returnHistory">{{ history }}</div>
    <div class="display" @click="addKeyCode">{{ answer }}</div>
    <button class="btn" @click="clear">AC</button>
    <button class="btn" @click="removeParameter"><=</button>
    <button class="btn" @click="addOperation('%')">%</button>
    <button class="btn" @click="addOperation('/')">/</button>
    <button class="btn" @click="addParameter(7)">7</button>
    <button class="btn" @click="addParameter(8)">8</button>
    <button class="btn" @click="addParameter(9)">9</button>
    <button class="btn" @click="addOperation('x')">x</button>
    <button class="btn" @click="addParameter(4)">4</button>
    <button class="btn" @click="addParameter(5)">5</button>
    <button class="btn" @click="addParameter(6)">6</button>
    <button class="btn" @click="addOperation('-')">-</button>
    <button class="btn" @click="addParameter(1)">1</button>
    <button class="btn" @click="addParameter(2)">2</button>
    <button class="btn" @click="addParameter(3)">3</button>
    <button class="btn" @click="addOperation('+')">+</button>
    <button class="btn" @click="addParameter('.')">.</button>
    <button class="btn" @click="addParameter(0)">0</button>
    <button class="btn" @click="addBraket">()</button>
    <button class="btn" @click="result">=</button>
  </div>
</template>

<script>
export default {
  data: () => ({
    answer: "",
    history: "80x9x7",
    bracket: false,
    operation: true
  }),
  methods: {
    async addKeyCode(event) {
      console.log(event);
      const code = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "."];
      const operation = ["-", "+", "/"];
      if (code.includes(event.key)) {
        await this.addParameter(event.key);
      } else if (operation.includes(event.key)) {
        await this.addOperation(event.key);
      } else if (event.key === "*") {
        await this.addOperation("x");
      } else if (event.key === "Backspace") {
        console.log('sadas')
        await this.removeParameter();
      } else if (event.key === "Enter") {
        console.log("xxx", this.answer);
        await this.result();
      }
    },
    returnHistory() {
      this.answer = this.history;
    },
    clear() {
      this.answer = "";
      this.bracket = false;
    },
    async result() {
      const mutiple = await this.answer.split("x");
      const divide = await this.answer.split("/");
      const plus = await this.answer.split("+");
      const minus = await this.answer.split("-");
      const percent = await this.answer.split("%");
      console.log(this.answer);
      console.log(mutiple, divide, plus, minus);
      console.log(mutiple[0] * mutiple[1]);
      if (
        mutiple.length == 2 &&
        divide.length == 1 &&
        plus.length == 1 &&
        minus.length == 1
      ) {
        this.history = this.answer;
        this.answer = mutiple[0] * mutiple[1];
      } else if (
        plus.length == 2 &&
        divide.length == 1 &&
        mutiple.length == 1 &&
        [1, 2].includes(minus.length)
      ) {
        this.history = this.answer;
        this.answer = Number(plus[0]) + Number(plus[1]);
        console.log(Number(plus[0]) + Number(plus[1]));
      } else if (
        divide.length == 2 &&
        mutiple.length == 1 &&
        plus.length == 1 &&
        minus.length == 1
      ) {
        this.history = this.answer;
        this.answer = Number(divide[0]) / Number(divide[1]);
      } else if (
        minus.length == 2 &&
        divide.length == 1 &&
        plus.length == 1 &&
        mutiple.length == 1
      ) {
        this.history = this.answer;
        this.answer = Number(minus[0]) - Number(minus[1]);
      } else if (percent.length == 2) {
        this.history = this.answer;
        this.answer = Number(percent[0]) * Number(percent[1]) / 100;
      }
    },
    removeParameter() {
      console.log(this.answer)
      this.answer = this.answer.toString().slice(0, -1);
    },
    addParameter(value) {
      this.answer += value.toString();
      if (this.operation === false) this.operation = true;
    },
    addOperation(value) {
      if (this.operation === true) {
        this.answer += value.toString();
        this.operation = false;
      }
    },
    addBraket() {
      const numberCount = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0];
      if (
        numberCount.includes(
          parseInt(this.answer.charAt(this.answer.length - 1))
        )
      ) {
        this.answer += ")";
        // this.bracket = true;
      } else {
        this.answer += "(";
        // this.bracket = false;
      }
    }
  }
};
</script>

<style>
.calculate {
  margin: auto;
  width: 500px;
  height: 500px;
  font-size: 50px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(7, 70px);
}
.display {
  grid-column: 1/5;
  background-color: whitesmoke;
  border: 50px;
  text-align: right;
}
.history {
  opacity: 0.4;
  grid-column: 1/5;
  font-size: 30px;
  text-align: right;
}
.btn {
  border: 1 px;
  text-align: center;
  font-size: 25px;
  background-color: rgb(248, 233, 247);
  outline: none;
  /* border: none;  */
  cursor: pointer;
}
</style>