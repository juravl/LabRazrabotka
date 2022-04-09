<template>
  <Page class="weather" @loaded="loaded">
    <ActionBar title="DropDown">
      <StackLayout orientation="horizontal">
        <Button class="back" text="Назад" @tap="$navigateBack()" />
      </StackLayout>
    </ActionBar>
    <FlexboxLayout justifyContent="flext-start" flexDirection="column">
      <FlexboxLayout alignSelf='center' justifyContent="space-around"  flexDirection="column">
          <Label alignSelf='center' class="city-name" text='Ханты-Мансийск' />
          <Label alignSelf='center' class='temp' :text="temp + ' \xB0C'" />
      </FlexboxLayout>
      <Image v-if="sky==='sun'" alignSelf='center' width=100 height=100 src="~/images/sun.png" stretch="aspectFit"/>
      <Image v-if="sky==='cloudy-sun'" alignSelf='center' width=100 height=100 src="~/images/cloudy-sun.png" stretch="aspectFit"/>
      <Image v-if="sky==='clouds'" alignSelf='center' width=100 height=100 src="~/images/clouds.png" stretch="aspectFit"/>
    <FlexboxLayout alignSelf='center' flexDirection="column">
        <Label class="pressure" :text="'Давление: ' + pressure + ' ммРт'" />
        <Label class="humidity" :text="'Влажность: ' + humidity + '%'" />
        <Label class="wind-speed" :text="'Скорость ветра: ' + wind.speed + ' м/с'" />
    </FlexboxLayout>
    </FlexboxLayout>
  </Page>
</template>

<script lang="ts">
  import Vue from "nativescript-vue";
  import {Http} from '@nativescript/core';

  export default Vue.extend({
    data() {

      let store = {
        temp: 0,
        pressure: 0,
        humidity: 0,
        wind: {
          "speed": 0,
          "deg": 0,
          "gust": 0,
        },
        sky: 'sun',
      }
      return store;
    },
    

    methods: {
      loaded() {
        console.log('load');
        Http.getJSON("https://api.openweathermap.org/data/2.5/weather?lat=61.007270&lon=69.030395&appid=6fadf16c35cb8b77ac1ff962ef1b67cf").then((res: any) => {
          // console.log(res);
          this.$data.temp = Math.round(res.main.temp - 273.15);
          this.$data.pressure = Math.round(res.main.pressure * 0.75);
          this.$data.humidity = res.main.humidity;
          this.$data.wind = res.wind;
          res.clouds.all = 50;
          if (res.clouds.all < 30) {
            this.$data.sky = 'sun'
          } else if (res.clouds.all < 60) {
            this.$data.sky = 'cloudy-sun'
          } else {
            this.$data.sky = 'clouds'
          }
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

.city-name {
  @extend %base
}

.temp {
  @extend %base;
}
.wind-speed {
  @extend %base;
  font-size: 25;
}
.humidity {
  @extend %base;
  font-size: 25;
}
.pressure {
  @extend %base;
  font-size: 25;
}
</style>
