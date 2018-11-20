<template>
  <div id="app">
    <section class="result">{{ current }}</section>
    <div @click="clear" class="clear">C</div>
    <div @click="revertNum" class="revertNum">+/-</div>
    <div @click="percent" class="percent">%</div>
    <div @click="addOperator('/')" class='orange divison'>÷</div>
    <div @click="addNum('7')" class="num-7">7</div>
    <div @click="addNum('8')" class="num-8">8</div>
    <div @click="addNum('9')" class="num-9">9</div>
    <div @click="addOperator('*')" class='orange multiplication'>x</div>
    <div @click="addNum('4')" class="num-4">4</div>
    <div @click="addNum('5')" class="num-5">5</div>
    <div @click="addNum('6')" class="num-6">6</div>
    <div @click="addOperator('-')" class='orange subtraction'>–</div>
    <div @click="addNum('1')" class="num-1">1</div>
    <div @click="addNum('2')" class="num-2">2</div>
    <div @click="addNum('3')" class="num-3">3</div>
    <div @click="addOperator('+')" class='orange addition'>+</div>
    <div @click="addNum('0')" class="num-0">0</div>
    <div @click="addDot('.')" class="dot">.</div>
    <div @click="equal" class='orange equal'>=</div>
  </div>
</template>


<script>
export default {
  name: 'app',
  data() {
    return {
      current: '0',
      temporary: null,
      operator: null,
      done: false,
      continue: false,
      toSum: true
    }
  },
  methods: {
    addNum(num) {
      if(this.current == '0') this.current = '';
      if(this.done) {
        this.current = '';
        this.temporary = null;
        this.operator = null;
        this.done = false;
      }
      if(!this.toSum) {
        this.current = '';
        this.toSum = true;
      }
      this.current = `${this.current}${num}`;
    },
    addOperator(operator) {
      if(!this.continue) {
        this.temporary = this.current;
        this.current = '0';
        this.operator = operator;
        this.done = false;
        this.continue = true;
      }
      else {
        if(this.toSum) {
          // Switch current and temporary
          this.switch();
          this.sum();
          this.temporary = this.current;
        }
        this.operator = operator;
        this.toSum = false;
      }
    },
    equal() {
      if(this.temporary) {
        if(!this.done) {
          // Switch current and temporary
          this.switch();
        }

        this.sum();

        this.done = true;
        this.continue = false;
      }
    },
    clear() {
      if(this.done) {
        this.current = '0';
        this.continue = false;
        this.done = false;
        this.operator = null;
        this.temporary = null;
      }
      this.current = '0';
    },
    revertNum() {
      if(this.current != '0') this.current = this.current*-1;
    },
    percent() {
      this.current = `${Number(this.current) / 100}`;
    },
    addDot(dot) {
      if(!this.current.includes(dot)) this.current = `${this.current}${dot}`;
    },
    sum() {
      switch(this.operator) {
        case '+':
          this.current = Number(this.current) + Number(this.temporary);
          break;
        case '-':
          this.current = Number(this.current) - Number(this.temporary);
          break;
        case '*':
          this.current = Number(this.current) * Number(this.temporary);
          break;
        case '/':
          this.current = Number(this.current) / Number(this.temporary);
          break;
      }
    },
    switch() {
      let temp = this.temporary;
      this.temporary = this.current;
      this.current = temp;
    }
  },
  mounted() {
    const functions = {
      '0':'addNum', 
      '1':'addNum', 
      '2':'addNum', 
      '3':'addNum', 
      '4':'addNum', 
      '5':'addNum',
      '6':'addNum',
      '7':'addNum',
      '8':'addNum',
      '9':'addNum',
      '/':'addOperator',
      '*':'addOperator',
      '-':'addOperator',
      '+':'addOperator',
      '.':'addDot',
      'c':'clear',
      'r':'revertNum',
      '%':'percent',
      'Enter':'equal'
    }

    window.addEventListener('keydown', (e) => {
      if(e.key in functions) {
        this[functions[e.key]](e.key);
      }
    })
  }
}
</script>


<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    font-family: Helvetica;
    background: radial-gradient(#33ccff 40%, #ff99cc);
}

#app {
    width: 25vw;
    height: 35vw;

    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(7 , 1fr);
    grid-gap: 2px;

    border-radius: 10px;
    overflow: hidden;
    user-select: none;
    -moz-user-select: none;
    box-shadow: 0 20px 20px 20px rgba(0, 0, 0, .13);
}

#app .result {
    grid-column: 1 / -1;
    grid-row: span 2;

    display: flex;
    justify-content: flex-end;
    align-items: flex-end;
    
    background-color: #202020;
    color: #fff;
    font-size: 4.5vw;
    padding: 0 1vw;
    font-weight: 200;
    overflow: hidden;
}

#app div {
    background-color: #c5c6ca;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 2.5vw;
    cursor: pointer;
}

#app div:hover {
    background-color: #adaeb1;
}

#app .orange {
    background-color: orange;
    color: #fff;
}

#app .orange:hover {
    background-color: rgb(238, 159, 11);
}

.num-0 {
    grid-column: span 2;
    padding-right: 50%;
}

@media only screen and (max-width: 1000px) {
    #app {
        width: 45vw;
        height: 60vw;
    }

    #app .result {
        font-size: 8.5vw;
    }

    #app div {
        font-size: 4.5vw;
    }
}

@media only screen and (max-width: 500px) {
    #app {
        width: 100vw;
        height: 100vh;
        border-radius: 0;
    }

    #app .result {
        font-size: 15.5vw;
    }

    #app div {
        font-size: 7.5vw;
    }
}
</style>
