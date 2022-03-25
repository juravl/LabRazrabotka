<template>
  <Page class='main-page'>
    <ActionBar>
      <DockLayout>
        <Label style="font-size: 20" dock="left" text="Конвертер"/>
      </DockLayout>
    </ActionBar>
    <Label text="123"/>
    <StackLayout   flexWrap="wrap">
      <Button class="base-btn" text="Валюты" @tap="$navigateTo(currecy)" />
      <Button class="base-btn" text="Температура" @tap="$navigateTo(temperature)" />
      <Button class="base-btn" text="Масса" @tap="$navigateTo(weight)" />
      <Button class="base-btn" text="Расстояние" @tap="$navigateTo(distance)" />
      <Button class="base-btn" text="Углы" @tap="$navigateTo(angle)" />
    </StackLayout  >
  </Page>
</template>

<script lang="ts">
  import Vue from "nativescript-vue";
  import Currecy from './Currecy.vue';
  import Weight from './Weight.vue';
  import Temperature from './Temperature.vue';
  import Distance from './Distance.vue';
  import Angle from './Angle.vue';

  export default Vue.extend({
    data() {
      return {
        'currecy': Currecy,
        'weight': Weight,
        'temperature': Temperature,
        'distance': Distance,
        'angle': Angle,
      }
    },
    computed: {
      message() {
        return "Blank {N}-Vue app";
      }
    }
  });
</script>

<style scoped lang="scss">
  @import '@nativescript/theme/scss/variables/blue';

  // Custom styles
  .fas {
    @include colorize($color: accent);
  }

  .info {
    font-size: 20;
    horizontal-align: center;
    vertical-align: center;
  }
  
</style>
