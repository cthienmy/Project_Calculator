<template>
  <div class="border1">
    <div class="border2">
      <div class="out">
        <div class="out1">
          <input class="out2" v-model="outNumber" disabled />
          <input class="operator" v-model="outOperator" disabled />
        </div>
      </div>
      <!--  -->
      <div>
        <div class="in">
          <!--  -->
          <numberIn
            v-bind:item="item"
            v-for="(item, index) in arritem"
            v-bind:key="index"
            v-on:outputNumber="outputNumber($event)"
          />
          <!--  -->
          <button
            style="background-color: rgb(224, 131, 57)"
            v-on:click="result($event)"
          >
            <div style="background-color: rgb(249, 151, 52)">=</div>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import numberIn from "./components/numberIn.vue";
export default {
  name: "app",
  components: {
    numberIn,
  },
  data() {
    return {
      arritem: [7, 8, 9, "/", 4, 5, 6, "*", 1, 2, 3, "-", 0, ".", "+"],
      outNumber: "0",
      outOperator: "",
      a: "",
      b: "",
      outResult: "",
    };
  },
  methods: {
    outputNumber: function (inputItem) {
      if (
        (typeof inputItem === "number" || inputItem === ".") &&
        this.outOperator === "" &&
        // là 1 số or "." và trước khi ấn toán tử -> gán cho a
        this.outResult === ""
        // truoc khi an '=', outResult === ''
      ) {
        if (this.a === "" && inputItem === ".") {
          return;
          //chưa nhập số thì không đc '.'
        }
        this.outNumber = inputItem;
        this.a = this.a + this.outNumber;
        //  cộng chuỗi để nối các kí tự số đơn thành 1 số có nhiều chữ số
        this.outNumber = this.a;
        console.log("a:", this.a);
        console.log("outNumber:", this.outNumber);
        console.log("outResult:", this.outResult);
      } else if (
        typeof inputItem === "number" &&
        this.outOperator === "" &&
        // là 1 số or "." và sau khi ấn toán tử->gán cho b
        this.outResult !== ""
      ) {
        if (inputItem === "." && this.outResult !== "") {
          return;
          // nếu sau khi ấn dấu '=' (this.outResult !== ""),
          //  mà kí tự tiếp theo là '.' thì ko làm gì cả
        }
        this.outResult = "";
        // sau khi an '=',outResult !== '', luc nay reset kq === '' roi bat dau tinhs phep tinh moi
        this.outNumber = inputItem;
        this.a = this.outResult + this.outNumber;
        this.outNumber = this.a;
        console.log("outResult:", this.outResult);
      } else if (
        (typeof inputItem === "number" || inputItem === ".") &&
        this.outOperator !== ""
      ) {
        this.outNumber = inputItem;
        this.b = this.b + this.outNumber;
        this.outNumber = this.b;
        console.log("b:", this.b);
        console.log("outNumber:", this.outNumber);
      } else if (typeof inputItem !== "number" && inputItem !== ".") {
        ///////// làm cho khi chưa ấn số thì dấu bị vô hiệu hóa???
        if (this.a === "") {
          return;
        }
        this.outOperator = inputItem;
        console.log("outOperator:", this.outOperator);
      }
    },
    result: function () {
      switch (this.outOperator) {
        case "+":
          this.a = Number(this.a) + Number(this.b);
          // phep cong js ko tu ep kieu sang Number nen ta phai tu chuyen doi
          this.outNumber = this.a;
          this.outResult = this.a;
          // ket qua sau khi an = thi gan cho 1 bien de danh dau ket qua do.

          this.b = "";
          // reset b lại ="", để sau khi cộng tiếp 1 số mới thì
          // biến b không cộng tạo chuỗi lại chính nó
          // rồi mới cộng tiếp (this.b = this.b + this.outNumber);
          this.outOperator = "";
          break;
        case "-":
          this.a = this.a - this.b;
          this.outNumber = this.a;
          this.outResult = this.a;
          this.b = "";
          this.outOperator = "";
          break;
        case "*":
          this.a = this.a * this.b;
          this.outNumber = this.a;
          this.outResult = this.a;
          this.b = "";
          this.outOperator = "";
          break;
        case "/":
          this.a = this.a / this.b;
          this.outNumber = this.a;
          this.outResult = this.a;
          this.b = "";
          this.outOperator = "";
          break;
      }
    },
  },
};
</script>

<style>
.border1 {
  background-color: rgb(81, 82, 89);
  border-radius: 30px;
  padding-bottom: 15px;
}
.border2 {
  background-color: rgb(97, 99, 105);
  width: 230px;
  padding: 30px;
  border-radius: 30px 30px 25px 25px;
}
.out {
  margin-bottom: 20px;
  display: flex;
  justify-content: center;
  width: 100%;
}
.out1 {
  background-color: rgb(195, 200, 212);
  padding-bottom: 8px;
  border-radius: 8px;
  width: 100%;
  position: relative;
}
.out2 {
  width: 100%;
  height: 40px;
  background-color: rgb(208, 213, 225);
  border-radius: 8px 8px 6px 6px;
  font-size: 40px;
  font-weight: 400;
  /* line-height: 48px; */
  text-align: right;
  padding: 0 8px;
  box-sizing: border-box;
  border: none;
  outline: none;
}
.operator {
  position: absolute;
  right: 0px;
  border: none;
  top: 0px;
  width: 9px;
  height: 9px;
  padding: 3px 0px 0px 2px;
  font-size: 12px;
  background: rgba(255, 255, 255, 0);
}
.in {
  display: grid;
  grid-template-columns: auto auto auto auto;
  gap: 8px;
}
</style>
