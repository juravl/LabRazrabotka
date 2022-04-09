<template>
  <Page class='main-page'>
    <ActionBar>
      <DockLayout>
        <Label style="font-size: 20" dock="left" text="Networking"/>
      </DockLayout>
    </ActionBar>
    <Label text="123"/>
    <StackLayout   flexWrap="wrap">
      <Button class="base-btn" text="Погода" @tap="$navigateTo(weather)" />
      <Button class="base-btn" text="Курсы валют" @tap="$navigateTo(wallet)" />
      <Button class="base-btn" text="Расписание" @tap="$navigateTo(timetable)" />
    </StackLayout  >
  </Page>
</template>

<script lang="ts">
  import Vue from "nativescript-vue";
  import Weather from './Weather.vue';
  import Wallet from './Wallet.vue';
  import Timetable from './Timetable.vue';

  export default Vue.extend({
    data() {
      return {
        'weather': Weather,
        'timetable': Timetable,
        'wallet': Wallet,
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
