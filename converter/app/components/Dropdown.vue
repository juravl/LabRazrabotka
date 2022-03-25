<template>
  <Page>
    <ActionBar title="DropDown">
      <StackLayout orientation="horizontal">
        <Button text="Back to Master" @tap="$modal.close('Не выбрано')" />
      </StackLayout>
    </ActionBar>
    <FlexboxLayout flexDirection="column">
      <ListView for="item in itemsList" @itemTap="selectItem">
          <v-template>
          <Label class="list" :text="item" />
        </v-template>
      </ListView>
      <Button text="Закрыть" @tap="selectItem({item: 'Не выбрано'})"/>
    </FlexboxLayout>
  </Page>
</template>

<script lang="ts">
  import Vue from "nativescript-vue";

  export default Vue.extend({
    props: ['items'],
    data() {
      return {
        itemsList: this.$props['items']
      }
    },

    methods: {
      selectItem(event: any) {
        console.log(event.item)
        this.$modal?.close(event.item)
      },
    },
  });
</script>

<style scoped lang="scss">
.list {
  font-size: 20;
}

</style>
