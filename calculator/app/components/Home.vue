<template>
  <Page>
    <ActionBar title="Master">
      <StackLayout>
        <!-- <Button text="To Details directly" actionBarHidden="true" @tap="$navigateTo(Vallet)" /> -->
      </StackLayout>
    </ActionBar>
  </Page>
</template>

<script lang="ts">
  import Vue from "nativescript-vue";
  // import Vallet from "../../../Vallet.vue";

  

  export default Vue.extend({
    data() {
      
      return {
        // "Vallet": Vallet,
      };
    },
    
    methods: {}
      
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
