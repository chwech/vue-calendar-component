<template>
  <div id="app">
    <!-- 日历头 -->
    <ul class="header">
      <li class="item" v-for="(th, i) of headers" :key="i">
        {{ th }}
      </li>
    </ul>
    <ul class="header" @click="handleClick">
      <li
        :data-date="`${date.year},${date.month},${date.day}`"
        :class="{ active: date.day === today.day && 
          date.month === today.month && 
          date.year === today.year }" 
        class="item" v-for="(date, i) of list" :key="i">
        {{ date.day }}
      </li>
    </ul>
    <p>{{ currentMonth + 1 }}月</p>
    <button @click="prev">上个月</button>
    <button @click="next">下个月</button>
  </div>
</template>

<script>

export default {
  name: 'app',
  components: {
  },
  data () {
    return {
      headers: ['日', '一', '二', '三', '四', '五', '六'],
      list: [],
      currentMonth: '',
      today: null
    }
  },
  methods: {
    handleClick (e) {
      // eslint-disable-next-line
      console.log(e.target.dataset.date)
      this.$emit('click-day', e.target.dataset.date)
    },
    prev () {
      let { year } = this.getToday()
      this.calc(year, --this.currentMonth)
    },
    next () {
      let { year } = this.getToday()
      this.calc(year, ++this.currentMonth)
    },
    calc (year, month) {
      // 当月1号
      let firstDayOfMonth = new Date(year, month, 1)
      let dayOfWeek = firstDayOfMonth.getDay()
      let start = firstDayOfMonth.getTime() - dayOfWeek * 24 * 60 * 60 * 1000
      this.list = []
      for (let i = 0; i < 42; i++) {
        let d = new Date(start + i * 24 * 60 * 60 * 1000)
        this.list.push({
          date: d,
          year: d.getFullYear(),
          month: d.getMonth(),
          day: d.getDate()
        })
      }
    },
    init () {
      let { year, month } = this.today = this.getToday()
      this.currentMonth = month
      this.calc(year, month)
    },
    getToday () {
      let date = new Date()
      let year = date.getFullYear()
      let month = date.getMonth()
      let day = date.getDate()
      return {
        date,
        year,
        month,
        day
      }
    }
  },
  created () {
    this.init()
  }
}
</script>

<style>
.header {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  width: 280px;
}
.item {
  width: 40px;
  height: 40px;
  text-align: center;
  line-height: 40px;
}
.active {
  color: #fff;
  background: #2679FE;
  border-radius: 50%;
}
</style>
