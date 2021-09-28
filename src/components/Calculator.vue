<template>
  <div class="calc">
    <h1 class="title">calculator</h1>
  
    <div class="result box" style="text-align: right">{{showNum}}</div>
      <div class="clear">
      <button @click="clearCurrent" class="button is-danger is-medium">C</button>
      <button @click="clearAll" class="button is-danger is-medium">AC</button>
      </div>
      <div class="buttons columns">
          <div class="number-buttons colum is-one-third">
            <button
              v-for="num in buttons.num"
              :key="num"
              class="button is-dark"
              @click="selectNumber(num)"
            >
              {{ num }}
            </button>
          </div>
          <div class="column">
            <button
              v-for="operator in buttons.operator"
              :key="operator"
              class="button is-primary is-medium"
              @click="selectOparator(operator)"
            >
              {{ operator }}
            </button>
          </div>
      </div>
      <div class="">
          <button @click="showResult" class="button is-success is-medium equal-button">=</button>
      </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      buttons: {
        num: [
          "7","8","9","4","5","6","1","2","3","0","00",".","+/-"
        ],
        operator: ["+", "-", "×","÷"]
      },
      isInsertNumber: true, // 数字入力中かどうかを判断
      isResult: false, // =を押した後かどうかを判断
      currentOperator: "", // 現在選択中の演算子
      currentNumber: "0", // 現在選択中の数字
      numArray: [], // 押した数字の配列を格納する
      opeArray: [] // 押した演算子の配列を格納する
    };
  },
  computed: {
    // 状態によって 合計値 or 入力値 を出し分ける
    showNum() {
      let result;
      // 数字が入力中なら次の入力値を出力して
      if (this.isInsertNumber) {
        let text = String(this.currentNumber);
        let newText = text.replace(/^0{1,}([^.])/, "$1");
        result = newText;
      } else {
        // そうでないならtotalメソッドの結果を返す
        result = this.total();
      }
      return result;
    }
  },
  methods: {
    // reduceメソッドを使い、全ての計算結果を出す
    total() {
      return this.numArray.reduce((prev, next, index) => {
        return this.setTotalNum(prev, next, index - 1);
      });
    },
    // 前後の数字を演算子ごとに計算して値を返す
    setTotalNum(prev, next, index) {
      if (this.opeArray[index] === "+") {
        return prev + next;
      } else if (this.opeArray[index] === "-") {
        return prev - next;
      } else if (this.opeArray[index] === "÷") {
        return prev / next;
      } else if (this.opeArray[index] === "×") {
        return prev * next;
      }
    },
    // 数字ボタンを押した時
    selectNumber(num) {
      // 正負を逆にする
      if (num === "+/-") {
        if (!this.isInsertNumber) return;
        this.currentNumber = -1 * this.currentNumber;
        return;
      }
      // = を押した直後、一度数字をリセットして次の計算をする
      if (this.isResult) {
        this.currentNumber = "0";
        this.isResult = false;
      }
      // 数字以外が入力された場合は演算子の配列に今の演算子をpushして数字0にし、次の数字を入力できるようにする
      if (!this.isInsertNumber) {
        this.opeArray.push(this.currentOperator);
        this.currentNumber = "0";
        this.isInsertNumber = true;
      }
      // 小数点は１つまでしか入力できない
      if (num === "." && this.currentNumber.indexOf(".") > -1) {
        return;
      }
      this.currentNumber += num;
    },
    // 演算子ボタンを押した時
    selectOparator(operator) {
      if (this.isInsertNumber) {
        this.numArray.push(Number(this.currentNumber));
        this.isInsertNumber = false;
      }
      this.currentOperator = operator;
    },
    // =を押した時に動くメソッド
    showResult() {
      // 数字入力時 かつ current演算子有り かつ まだ=ボタンを押していないとき
      // 入力された数字、演算子をリセットし、currentNumberにtotalメソッドで計算結果を返す
      if (this.isInsertNumber && this.currentOperator && !this.isResult) {
        this.numArray.push(Number(this.currentNumber));
        this.currentOperator = "";
        this.currentNumber = this.total();
        this.numArray = [];
        this.opeArray = [];
        this.isResult = true;
      }
    },
    // cを押した時
    clearCurrent() {
      this.currentNumber = 0;
      this.isInsertNumber = true;
    },
    // acを押した時
    clearAll() {
      this.numArray = [];
      this.opArray = [];
      this.currentNumber = 0;
      this.currentOperator = "";
      this.isInsertNumber = true;
    }
  }
};
</script>

<style>
.calc {
  width: 380px;
  padding: 20px 0;
  margin: 10px auto;
  background-color: rgba(122, 196, 199, 0.808);
  border-radius: 20px;
  box-shadow: 8px 5px rgba(62, 150, 145, 0.6);
}

.title {
  font-size: 20px;
  color: rgb(235, 227, 128);
  text-shadow: 1px 1px rgb(105, 95, 95);

}
.result {
  width: 250px;
  font-size: 30px;
  font-weight: bold;
  margin: 0 auto;
  padding:8px 10px;
  border: 1px solid;
  background-color: rgb(248, 165, 165);
  color: rgb(231, 220, 220);
  /* 数字が表示できない桁数になったら...で表示 */
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;


}
.clear {
  width: 300px;
  margin: 10px auto;
  display: flex;
}

.clear button {
  margin: 0 5px;
}

.buttons {
  width: 300px;
  margin: 0 auto 0;
}

.number-buttons button {
  width: 30%;
  margin: 0 0 5px;
  font-size: 15px;

}

.op-buttons button {
  width: 22%;
}

.equal-button {
  margin: 0 auto 0;
  width: 260px;
}
p { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }


</style>
