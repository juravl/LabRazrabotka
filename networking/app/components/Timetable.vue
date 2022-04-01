<template>
  <Page class="weather" @loaded="loaded">
    <ActionBar title="DropDown">
      <StackLayout orientation="horizontal">
        <Button class="back" text="Назад" @tap="$navigateBack()" />
      </StackLayout>
    </ActionBar>
    <ActivityIndicator :busy="isLoad" />
    <ScrollView v-if="!isLoad">
        <ListView for="item in itemsList">
            <v-template>
              <DockLayout>
                <FlexboxLayout flexDirection="column" class="item">
                  <Label class="text title" :text='timeRef[Number(item.PAIR) - 1]' />
                  <TextView class="text" editable='false' :text="item.DISCIPLINE" />
                  <Label class="text teacher" :text="item.TEACHER" />
                  <Label class="text" :text="'Каб. ' + item.AUD + '/' + item.KORP" />
                </FlexboxLayout>
              </DockLayout>
          </v-template>
        </ListView>
    </ScrollView>
  </Page>
</template>

<script lang="ts">
  import Vue from "nativescript-vue";
  import {Http} from '@nativescript/core';

  export default Vue.extend({
    data() {

      let store = {
        isLoad: true,
        itemsList: [],
        timeRef: ['8:30', '10:15', "12:30", "14:35", "16:20", "18:05"]
      }
      return store;
    },
    

    methods: {
      loaded() {
        console.log('load');
        Http.getJSON("http://timetable.ugrasu.ru/index.php/json?group=1181&date=02_04_2022").then((res: any) => {
          // console.log(res);
          this.$data.itemsList = res;
          this.$data.isLoad = false;
          
        }).catch(e => console.log(e))
      },
    },    
  });
</script>

<style scoped lang="scss">
%base {
  font-size: 30;
  color: antiquewhite;
}

.title {
  font-weight: bold;
}

.text {
  font-size: 20
}
.teacher {
  font-style: italic; 
}
.item {
  background: linear-gradient(#bdd7eb, #4497f7);
  margin: 5;
  color: black
}
</style>
