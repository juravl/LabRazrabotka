<template>
  <Page>
    <ActionBar>
      <Label text="Calculator"/>
    </ActionBar>

    <DockLayout class="calculator">
      <Label dock="top" class="display" :text="notzero"/>
      <FlexboxLayout dock="bottom" flexDirection="column" justifyContent="flex-end" class="layer">
        <FlexboxLayout  >
          <Button width="50%" text="C" @tap="clear" class="btn big btn_num"/>
          <Button text="^" @tap="pow" class="btn operator" v-bind:class="{'operator_dark':chek_clik('pow')}"/>
          <Button text="/" @tap="divide" class="btn operator" v-bind:class="{'operator_dark':chek_clik('divide')}"/>
        </FlexboxLayout>
        <FlexboxLayout class="layer">
          <Button text="7" @tap="append('7')" class="btn btn_num"/>
          <Button text="8" @tap="append('8')" class="btn btn_num"/>
          <Button text="9" @tap="append('9')" class="btn btn_num"/>
          <Button text="*" @tap="multiply" class="btn operator" v-bind:class="{'operator_dark':chek_clik('multiply')}"/>
        </FlexboxLayout> 
        <FlexboxLayout class="layer">
          <Button text="4" @tap="append('4')" class="btn btn_num"/>
          <Button text="5" @tap="append('5')" class="btn btn_num"/>
          <Button text="6" @tap="append('6')" class="btn btn_num"/>
          <Button text="-" @tap="minus" class="btn operator" v-bind:class="{'operator_dark':chek_clik('minus')}"/>
        </FlexboxLayout> 
        <FlexboxLayout class="layer">
          <Button text="1" @tap="append('1')" class="btn btn_num"/>
          <Button text="2" @tap="append('2')" class="btn btn_num"/>
          <Button text="3" @tap="append('3')" class="btn btn_num"/>
          <Button text="+" @tap="add" class="btn operator" v-bind:class="{'operator_dark':chek_clik('add')}"/>
        </FlexboxLayout> 
        <FlexboxLayout class="layer">
          <Button width="50%" text="0" @tap="append('0')" class="btn big btn_num"/>
          <Button text="." @tap="dot" class="btn btn_num"/>
          <Button text="=" @tap="equal" class="btn operator"/>
        </FlexboxLayout> 

      </FlexboxLayout>
      
    </DockLayout>
  </Page>
</template>

<script lang="ts">
  import Vue from "nativescript-vue";

  interface TData {
      previous: string;
      current: string;
      operator: (a: number, b: number)=>number;
      operatorClicked: boolean;
      operationname: string;
  }

  export default Vue.extend({
    data() {
      let store: TData = {
        previous: '',
        current: '',
        operator: (a, b)=>{return 0;},
        operatorClicked: false,
        operationname: ''
      }
      return store;
    },
    computed: {
      notzero() {
        try {
          return String(Number(this.$data.current || "0"))
        } catch (error) {
          return '0'
        }  
      },
      chek_clik() {
        return (opt: String) => {
          console.log(opt);
          return opt == this.operationname
        }
      }
    },

    methods: {
      clear() {
        this.current = '';
        this.operationname = '';
      },
      pow() {
        this.operator = (a, b) => Math.pow(a, b);
        this.setPrevious();
        this.operationname = 'pow';
      },
      append(number: string) {
        if (this.operatorClicked) {
          this.current = '';
          this.operatorClicked = false;
        }
        this.current = `${this.current}${number}`;
      },
      setPrevious() {
        this.previous = this.current;
        this.operatorClicked = true;
      },
      divide() {
        this.operator = (a, b) => {
          let result = a / b;
          return result != Infinity ? result:Infinity
          
        };
        this.setPrevious();
        this.operationname = 'divide';
      },
      multiply() {
        this.operator = (a, b) => a * b;
        this.setPrevious();
        this.operationname = 'multiply';
      },
      minus() {
        this.operator = (a, b) => a - b;
        this.setPrevious();
        this.operationname = 'minus';
      },
      add() {
        this.operator = (a, b) => a + b;
        this.setPrevious();
        this.operationname = 'add';
      },
      dot() {
      if (this.current.indexOf('.') === -1) {
        this.append('.');
      }
    },
      equal() {
        let res =this.operator(
          parseFloat(this.previous),
          parseFloat(this.current), 
        );


        this.current = String(res);
        this.previous = '';
        this.operationname = '';
      }
    }
  });
</script>

<style scoped lang="scss">
.btn_dark {
  background-color: rgb(223, 121, 37);
}
.calculator {
  margin: 0;
  padding: 0;
  width: 1000px;
  font-size: 100px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(100px, auto);
}
.display {
  background-color: #333;
  color: white;
  text-align: right;
}
.big {
  width: 50% !important;
}
.btn {
  width: 25%;
  margin: 10;
  padding: 0;
  background-color: #F2F2F2;
  border: 1px solid #999;
  font-size: 40px;
  border-radius: 100;
}
.btn_num:active {
  background-color: darken($color: #F2F2F2, $amount: 20);
}
.operator {
  background-color: orange;
  color: white;
}

.operator_tap:active {
  background-color: darken($color: orange, $amount: 20);
}

.operator_dark {
  background-color: darken($color: orange, $amount: 20);
}

.layer {
  width: 1000px;
  padding: 0;
  margin: 0;
}

Page {
  background-color: #333;
}

</style>
