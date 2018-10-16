<template>
  <div class="calendar">
    <div class="calendar-header">
      <i class="fas fa-angle-left icon" v-on:click="changeMonth(-1)"></i>
      <p>{{ year + '年 ' + month + '月' }}</p>
      <i class="fas fa-angle-right icon" v-on:click="changeMonth(1)"></i>
    </div>
    <div class="calendar-weekdays">
      <ul class="weekdays">
        <li v-for="(day, weekkey) in days" :key="weekkey">{{ day }}</li>
      </ul>
    </div>
    <div class="calendar-dates">
      <ul class="dates">
        <li v-for="(blank, key) in firstDayOfMonth" :key="`before${key}`" class="dateIsOver"></li>
        <li v-for="(date, key) in daysInMonth" :key="key" v-on:click="goEditTodo(date)">
          <p class="date" :class="{'today': getToday(date)}" v-text="date < 10 ? '0' + date : date"></p>
          <div class="holidays">
            <template v-for="(holiday, h) in holidays">
              <p class="holiday" v-if="holiday.start.date === `${year}-${month < 10 ? `0${month}` : month}-${date}`" :key="h"> {{ holiday.summary }} </p>
            </template>
          </div>
        </li>
        <li v-for="(blank, key) in endBlank" :key="`after${key}`" class="dateIsOver"></li>
      </ul>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import moment from 'moment'

export default {
  name: 'calendar',
  data () {
    return {
      today: moment(),
      dateContext: moment(),
      days: ['日', '一', '二', '三', '四', '五', '六'],
      nowMonth: '',
      holidays: []
    }
  },
  created () {
    this.nowMonth = this.month
    this.getHoliday()
  },
  methods: {
    goEditTodo (date) {
      const getDate = `${this.year}-${this.month < 10 ? '0' + this.month : this.month}-${date < 10 ? '0' + date : date}`
      this.$bus.$emit('showTodoList', getDate)
    },
    changeMonth (type = 1) {
      if (type === 1) {
        this.dateContext = moment(this.dateContext).add(1, 'month')
      } else {
        this.dateContext = moment(this.dateContext).subtract(1, 'month')
      }
      this.nowMonth = this.month
    },
    // 抓今天的日期
    getToday (date) {
      const todayMonth = this.today._d.getMonth() + 1
      const todayDate = this.today._d.getDate()
      // console.log(todayMonth, todayDate);
      if (todayMonth === this.nowMonth && todayDate === date) {
        return true
      } else {
        return false
      }
    },
    // 抓出google calendar api的所有台灣節日
    getHoliday () {
      const vm = this
      vm.holidays = []
      const getNowMonth = vm.nowMonth < 10 ? `0${vm.nowMonth}` : vm.nowMonth
      const nowDate = `${vm.year}-${getNowMonth}`
      console.log(nowDate)
      const country = 'taiwan'
      const apikey = 'AIzaSyD40d-1Xg8TrPqcfgZ03ZiHYLMde4Odzys'
      const api = `
        https://www.googleapis.com/calendar/v3/calendars/zh.${country}%23holiday%40group.v.calendar.google.com/events?key=${apikey}
      `
      this.axios.get(api).then((res) => {
        // console.log(res);
        if (res.status === 200) {
          res.data.items.forEach(holiday => {
            // if (holiday.start.date.indexOf(nowDate) !== -1) {
            //   console.log(holiday.start.date, nowDate, holiday.summary)
            //   switch (holiday.summary) {
            //     case '國慶日/雙十節彈性放假':
            //       holiday.summary = '雙十節'
            //       break
            //     case '台灣光復節':
            //       holiday.summary = '光復節'
            //       break
            //   }
            //   vm.holidays.push(holiday)
            // }
            switch (holiday.summary) {
              case '國慶日/雙十節彈性放假':
                holiday.summary = '雙十節'
                break
              case '台灣光復節':
                holiday.summary = '光復節'
                break
            }
            vm.holidays.push(holiday)
          })
        }
      })
    }
  },
  computed: {
    year () {
      const t = this
      return t.dateContext.format('Y')
    },
    month () {
      const t = this
      let month = t.dateContext.format('MMMM')
      switch (month) {
        case 'January':
          month = 1
          break
        case 'February':
          month = 2
          break
        case 'March':
          month = 3
          break
        case 'April':
          month = 4
          break
        case 'May':
          month = 5
          break
        case 'June':
          month = 6
          break
        case 'July':
          month = 7
          break
        case 'August':
          month = 8
          break
        case 'September':
          month = 9
          break
        case 'October':
          month = 10
          break
        case 'November':
          month = 11
          break
        case 'December':
          month = 12
          break
        default:
          month = 1
      }
      return month
    },
    daysInMonth () {
      const t = this
      return t.dateContext.daysInMonth()
    },
    currentDate () {
      const t = this
      return t.dateContext.get('date')
    },
    firstDayOfMonth () {
      const t = this
      const firstDay = moment(t.dateContext).subtract((t.currentDate - 1), 'days')
      return firstDay.weekday()
    },
    endBlank () { // 算每個月後面還缺幾個空格
      let blank = 35 - this.firstDayOfMonth - this.daysInMonth
      if (blank < 0) {
        blank = 42 - this.firstDayOfMonth - this.daysInMonth
      }
      // console.log(blank);
      return blank
    },
    initialDate () {
      const t = this
      return t.today.get('date')
    },
    initialMonth () {
      const t = this
      return t.today.format('MMMM')
    },
    initialYear () {
      const t = this
      return t.today.format('Y')
    }
  }
}
</script>

<style lang="sass" scoped>
@import 'src/assets/sass/global.sass'

.calendar
  +size(100%)
  padding: 0 10%
  .calendar-header
    +size(100%, 3rem)
    +center
    font-size: 1.5rem
    p
      margin: 0 5rem
  .calendar-weekdays
    +size(100%, 2rem)
    ul.weekdays
      +size(100%)
      +center
      li
        +size(calc(100% / 7), 100%)
        +center
  .calendar-dates
    width: 100%
    ul.dates
      width: 100%
      +center
      flex-wrap: wrap
      li
        +size(calc(100% / 7), 16vh)
        display: flex
        justify-content: flex-start
        align-items: flex-start
        flex-direction: column
        border-right: 1px solid #aaa
        border-bottom: 1px solid #aaa
        &:nth-child(7n + 1)
          border-left: 1px solid #aaa
        &:nth-child(1), &:nth-child(2), &:nth-child(3), &:nth-child(4),
        &:nth-child(5), &:nth-child(6), &:nth-child(7)
          border-top: 1px solid #aaa
    .date, .holidays
      +size(100%, auto)
      +center
      min-height: 1.2rem
      font-size: 0.8rem
    .date
      color: #808080
      &.today
        background-color: $primary
        color: #000
    .holidays
      letter-spacing: 1px
      .holiday
        +size(100%)
        +center
        background-color: #ff3333
        color: #fff
        text-align: center
</style>
