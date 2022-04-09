<template>
  <Page @loaded='loaded'>
    <ActionBar>
      <StackLayout orientation="horizontal">
      </StackLayout>
    </ActionBar>
    <FlexboxLayout flexDirection="column" >
      <StackLayout orientation="horizontal">
      <Button class="back" text='<' @tap='generateMetadata(getPrevent())'/>
      <Button class="back" text='>' @tap='generateMetadata(getNext())'/>
      <Button class="todayBut" text='Today' @tap='generateMetadata()'/>
    </StackLayout>
    <Label class="title text" :text="monthName + ' - ' + year"/>
    <FlexboxLayout height="80%" alignSelf="center" justifyContent="center">
      <GridLayout rows="auto, auto, auto, auto, auto, auto, auto" columns="auto, auto, auto, auto, auto, auto, auto">
        <Label class="item title text"
              row='0' 
              :col="key" 
              :text='val' v-for="val, key in weekDayNames" :key="key"/>
        <Label class="item title text" :class="{'today': day.isNow, 'hollyday': isHollyDay(day.weekDay)}"
              :row='day.weekNum + 1' 
              :col="day.weekDay" 
              :text='day.num' v-for="day in getWeeks()" :key="day.num"/>         
      </GridLayout>
    </FlexboxLayout>
    </FlexboxLayout>
  </Page>
</template>

<script lang="ts">
  import Vue from "nativescript-vue";
  import dayjs, { Dayjs } from 'dayjs';
  import * as _ from "lodash";

  export default Vue.extend({
    data() {
      const now = dayjs()
      
      let store = {
        now: now,
        monthName: '',
        year: 0,
        weeks: [],
        weekDayNames: ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс']
      }
      return store;
    },
    

    methods: {
      getAlign(key: number) {
        return key == 0 ? 'flex-end': 'fex-start'
      },
      
      getWeeks() {return _.flatMap(this.$data.weeks, (val) => val)},

      getMonthName(num: number) {return ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'][num]},

      isHollyDay(weekDay: number) {return weekDay == 5 || weekDay == 6},

      getNext() {return this.$data.now.add(1, 'month')},
      getPrevent() {return this.$data.now.subtract(1, 'month')},

      generateMetadata(now: dayjs.Dayjs|null) {
        if (now == null) {
          now = dayjs()
        }

        let weeks = []

        const today = dayjs()

      let startDate = now.startOf('month')
      let endDate = now.endOf('month')

      for (let day = 1; day <= endDate.date(); day++) {
        // console.log('new week', day);
        let currenDate = startDate.add(day - 1, 'day')
        let week = [];
        // console.log('week', day, currenDate.day(), currenDate.date());
        for (;day <= endDate.date() && currenDate.day() != 0; day++) {
          currenDate = startDate.add(day - 1, 'day')
          // console.log(day, currenDate.day(), currenDate.date());
          week.push({
            isNow: currenDate.isSame(today, 'day') && currenDate.isSame(today, 'month') && currenDate.isSame(today, 'year'),
            num: day,
            weekDay: currenDate.day() == 0 ? 6 : currenDate.day() - 1,
            weekNum: weeks.length,
          })
        }
        currenDate = startDate.add(day - 1, 'day')
        if (day <= endDate.date())
          week.push({
              isNow: currenDate.isSame(today, 'day') && currenDate.isSame(today, 'month') && currenDate.isSame(today, 'year'),
              num: day,
              weekDay: (currenDate.day() == 0 ? 6 : currenDate.day() - 1),
              weekNum: week.length == 0 ? 0 : weeks.length + 1,
            })
        weeks.push(week)
      }
      // console.log(weeks)
      // console.log(now.month())

      this.$data.weeks = _.sortedUniq(weeks);
      this.$data.monthName = this.getMonthName(now.month());
      this.$data.now = now;
      this.$data.year = now.year();
      },

      loaded() {
        this.generateMetadata(null)
      }
    },
  });
</script>

<style scoped lang="scss">
%base {
  font-size: 30;
  color: rgb(245, 240, 234);
}
.title {
  font-weight: bold;
}
.text {
  font-size: 22;
  text-align: center;
}
.item {
  margin: 10;
  width: 10%;
  height: 10%;
}
.today {
  background-color: rgb(226, 194, 194);
  align-content: center;
  text-align: center;
  border-radius: 95%;
}
.hollyday {
  color: red;
}
.back {
  font-size: 30;
  width: 80;
  outline: none;
  background-color: rgb(245, 237, 234);
  border-radius: 95%;
  margin: 1%;
}
.todayBut {
  font-size: 22;
  background-color: rgb(245, 237, 234);
  border-radius: 95%;
}
</style>
