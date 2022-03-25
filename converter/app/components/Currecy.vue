<template>
  <Page class='main-page'>
    <ActionBar title="Currecy">
      <StackLayout orientation="horizontal">
        <Button class="back" text="Назад" @tap="$navigateBack()" />
      </StackLayout>
    </ActionBar>
    <FlexboxLayout width="100%" flexDirection="column" justifyContent="space-between" class="calculator">
      <FlexboxLayout>
      <Label class="dropdown"  
      :text="valFrom"
      @tap="$showModal(DropDown, { fullscreen: false, props: {items: Object.keys(items)}}).then(data => {valFrom = data})"/>
      <Label dock="top" class="display" :text="notzero"/>
    </FlexboxLayout>
    
    <FlexboxLayout>
      <Label class="dropdown"
        :text="valTo"
        @tap="$showModal(DropDown, { fullscreen: false, props: {items: Object.keys(items)}}).then(data => {valTo = data})" />
      <Label dock="top" class="display" :text="converted"/>
      </FlexboxLayout>
      
      <FlexboxLayout dock="bottom" flexDirection="column" justifyContent="flex-end" class="layer">
        <FlexboxLayout  >
          <Button width="50%" text="C" @tap="clear" class="btn big btn_num"/>
          <Button width="50%" text="<" @tap="backspace" class="btn big btn_num"/>
        </FlexboxLayout>
        <FlexboxLayout class="layer" justifyContent="space-around">
          <Button text="7" @tap="append('7')" class="btn btn_num"/>
          <Button text="8" @tap="append('8')" class="btn btn_num"/>
          <Button text="9" @tap="append('9')" class="btn btn_num"/>
        </FlexboxLayout> 
        <FlexboxLayout class="layer" justifyContent="space-around">
          <Button text="4" @tap="append('4')" class="btn btn_num"/>
          <Button text="5" @tap="append('5')" class="btn btn_num"/>
          <Button text="6" @tap="append('6')" class="btn btn_num"/>
        </FlexboxLayout> 
        <FlexboxLayout class="layer" justifyContent="space-around"> 
          <Button text="1" @tap="append('1')" class="btn btn_num"/>
          <Button text="2" @tap="append('2')" class="btn btn_num"/>
          <Button text="3" @tap="append('3')" class="btn btn_num"/>
        </FlexboxLayout> 
        <FlexboxLayout class="layer">
          <Button width="50%" text="0" @tap="append('0')" class="btn big btn_num"/>
          <Button width="50%" text="." @tap="dot" class="big btn btn_num"/>
        </FlexboxLayout> 

      </FlexboxLayout>
      
    </FlexboxLayout>
  </Page>
</template>

<script lang="ts">
  import Vue from "nativescript-vue";
  import DropDown from './Dropdown.vue';

  export default Vue.extend({
    data() {

      let store = {
        DropDown: DropDown,
        previous: '',
        current: '',
        operator: (a: number, b: number)=>{return 0;},
        operatorClicked: false,
        operationname: '',
        valFrom: 'Рубль',
        valTo: 'Доллар',
        items: {
          'Доллар': [1, 1],
          'Рубль': [1/100, 100], 
          'Евро': [1/1.1, 1.1],
        },
        selectedIndex: 1,

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
      converted() {
        let valFrom = this.$data.items[this.$data.valFrom];
        let valTo = this.$data.items[this.$data.valTo];
        // console.log(valFrom, valTo);
        return valFrom[0]*Number(this.$data.current)*valTo[1];

      },
      
      chek_clik() {
        return (opt: String) => {
          // console.log(opt);
          return opt == this.operationname
        }
      }
    },

    methods: {
      clear() {
        this.current = '';
        this.operationname = '';
      },
      backspace(){
        this.current = this.current.slice(0, this.current.length-1)
      },
      append(number: string) {
        if (this.operatorClicked) {
          this.current = '';
          this.operatorClicked = false;
        }
        this.current = `${this.current}${number}`;
      },
      dot() {
      if (this.current.indexOf('.') === -1) {
        this.append('.');
      }
    },
    }
  });
</script>

<style scoped lang="scss">

</style>
