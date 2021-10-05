<template>
  <div class="calendar">
    <div class="month">
      <!-- 头部 20xx年xx月 -->
      <ul>
        <li
          class="arrow left"
          @click="pickPre(currentYear, currentMonth)"
        >
          <i class="el-icon-arrow-left" />
        </li>
        <li class="year-month">
          <span class="choose-year">{{ currentYear }}年</span>
          <span class="choose-month">{{ currentMonth }}月</span>
        </li>
        <li class="arrow right" @click="pickNext(currentYear, currentMonth)">
          <i class="el-icon-arrow-right" />
        </li>
      </ul>
    </div>
    <!-- 周几 -->
    <ul class="weekdays">
      <li>日</li>
      <li>一</li>
      <li>二</li>
      <li>三</li>
      <li>四</li>
      <li>五</li>
      <li>六</li>
    </ul>
    <!-- 具体某一天号数 -->
    <ul class="days">
      <template v-for="(day, $index) in days">
        <li
          v-if="
            day.getFullYear() < new Date().getFullYear() ||
              (day.getFullYear() == new Date().getFullYear() &&
                day.getMonth() < new Date().getMonth()) ||
              (day.getFullYear() == new Date().getFullYear() &&
                day.getMonth() == new Date().getMonth() &&
                day.getDate() < new Date().getDate())
          "
          :key="$index"
          class="unDate unActive"
          @click="pick(day, $index)"
        >
          {{ day.getDate() }}
        </li>
        <li
          v-else-if="day.getMonth() + 1 != currentMonth"
          :key="$index"
          :class="classObject(day)"
          class="other-month"
          @click="pick(day, $index)"
        >
          <span>{{ day.getDate() }} </span>
        </li>
        <li
          v-else-if="
            day.getFullYear() == new Date().getFullYear() &&
              day.getMonth() == new Date().getMonth() &&
              day.getDate() == new Date().getDate()
          "
          :key="$index"
          :class="classObject(day)"
          @click="pick(day, $index)"
        >
          <span class="curActive">{{ day.getDate() }} </span>
        </li>
        <li
          v-else
          :key="$index"
          :class="classObject(day)"
          @click="pick(day, $index)"
        >
          <span>{{ day.getDate() }} </span>
        </li>
      </template>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    monthDay: {
      type: Array,
      default: function() {
        return []
      }
    },
    activeDay: {
      type:Date,
      default: ()=>{
          return new Date()
      }
    },
    defaultSelect: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      currentDay: 1,
      currentMonth: 1,
      currentYear: 1970,
      currentWeek: 1,
      days: [],
      isActive: -1,
      unActive: true,
      selectDay: this.activeDay
    }
  },
  watch: {
    // defaultSelect: function(val, oldVal) {},
    activeDay: function(val, oldVal) {
      this.selectDay = this.activeDay
      this.initData(
        this.formatDay(
          this.selectDay.getFullYear(),
          this.selectDay.getMonth() + 1,
          this.selectDay.getDate()
        )
      )
      console.log(val,oldVal,'val oldVal')
    }
  },
  created() {
    this.initData(null)
  },
  methods: {
    //设置返回样式对象
    // 当日 以及 monthDay [] 某几天 可约 的样式
    classObject: function(day) {
      let dayStr = this.formatDay(
        day.getFullYear(),
        day.getMonth() + 1,
        day.getDate()
      )
      let mActive = false
      for (let i = 0; i < this.monthDay.length; i++) {
        if (dayStr === this.monthDay[i]) {
          mActive = true
          break
        }
      }
      let oActive = false
      let sStop = false


      return {
        active:
          day.getFullYear() === this.selectDay.getFullYear() &&
          day.getMonth() === this.selectDay.getMonth() &&
          day.getDate() === this.selectDay.getDate(),
        monthActive: mActive,
        otherActive: oActive,
        stop: sStop
      }
    },
    // 传入时间格式  2021/09/30
    initData: function(cur) {
      let date
      if (cur) {
        date = new Date(cur)
      } else {
        date = new Date()
      }
      //      console.log(date);
      this.currentDay = date.getDate()
      this.currentYear = date.getFullYear()
      this.currentMonth = date.getMonth() + 1
      //      this.currentWeek = date.getDay(); // 1...6,0
      this.days.length = 0
      let firstStr = this.formatDate(this.currentYear, this.currentMonth, 1)
      let firstDate = new Date(firstStr)
      console.log(firstDate,'firstDate')
      let firstWeek = firstDate.getDay() //获得周几

      if (firstWeek === 0) {
        firstWeek = 7
      }

      for (let i = firstWeek; i > 0; i--) {
        let d = new Date(firstDate)
        d.setDate(d.getDate() - i)
        this.days.push(d)
      }

      for (let i = 0; i < 42 - firstWeek; i++) {
        let d = new Date(firstDate)
        d.setDate(d.getDate() + i)

        this.days.push(d)
      }
      // console.log(this.days,'this.days..... 198')

    },
    pick: function(date, index) {
      console.log(date,index,'pick选中事件...')
      if (!this.defaultSelect) {
        this.$emit('change', date)
        return
      }

      let d = new Date()
      let dateStr = this.formatDate(
        date.getFullYear(),
        date.getMonth() + 1,
        date.getDate()
      )
      let dStr = this.formatDate(d.getFullYear(), d.getMonth() + 1, d.getDate()) //当前时间戳

      //  选中的时间戳 和 当前时间的时间戳 比较
      //  ps:选了过去的时间
      if (Date.parse(dateStr) < Date.parse(dStr)) {
        console.log('220')
        return
      }


      // 选中的月份 和 当前的月份不等  并且 选中的时间戳 > 当前时间戳 (选了将来的时间)
      // ps:选了将来的时间 (重新更新日历数据 使其日历数据换到 将来月份)
      if (
        date.getMonth() + 1 !== this.currentMonth &&
        Date.parse(dateStr) >= Date.parse(dStr)
      ) {
        let dayStr = this.formatDate(
          date.getFullYear(),
          date.getMonth() + 1,
          date.getDate()
        )
        this.initData(dayStr)
      }
      this.selectDay = date
      this.$emit('change', date)
    },
    pickPre: function(year, month) {
      // setDate(0); 上月最后一天
      // setDate(-1); 上月倒数第二天
      // setDate(dx) 参数dx为 上月最后一天的前后dx天
      const self = this

      // let d = new Date()
      // let dateStr = self.formatDate(year, month, 1)
      // let dStr = self.formatDate(d.getFullYear(), d.getMonth() + 1, d.getDate())

       var preDate = new Date(self.formatDate(year, month, 1))
        preDate.setDate(0)
        self.$emit('pickPre', preDate.getFullYear(), preDate.getMonth() + 1)
        self.initData(
          self.formatDate(preDate.getFullYear(), preDate.getMonth() + 1, 1)
        )

    },
    pickNext: function(year, month) {
      const self = this
      var d = new Date(self.formatDate(year, month, 1))
      d.setDate(35) // 超过30或者31指的是下一个月  如果直接月份+1 到12月年份会出问题
      self.$emit('pickNext', d.getFullYear(), d.getMonth() + 1)
      self.initData(self.formatDate(d.getFullYear(), d.getMonth() + 1, 1))
    },
    formatDate: function(year, month, day) {
      // 返回 类似 2016/01/02 格式的字符串
      var y = year
      var m = month
      if (m < 10) m = '0' + m
      var d = day
      if (d < 10) d = '0' + d
      return y + '/' + m + '/' + d
    },
    formatDay: function(year, month, day) {
      // 返回 类似 2016-01-02 格式的字符串
      var y = year
      var m = month
      if (m < 10) m = '0' + m
      var d = day
      if (d < 10) d = '0' + d
      return y + '-' + m + '-' + d
    }
  }
}
</script>

<style scoped>
.calendar {
  box-sizing: border-box;
  font-weight: 600;
  width: 100%;
  color: #737e8b;
  font-size: 14px;
}

ul {
  list-style-type: none;
  box-sizing: border-box;
}

.month {
  line-height: 0px;
  width: 100%;
  background: #fff;
}

.month ul {
  margin: 0;
  padding: 0;
  display: inline-block;
  text-align: center;
  width: 100%;
  border-bottom: 1px solid #ebeef5;
}

.year-month {
  display: inline;
  line-height: 42px;
  color: #25303d;
  font-size: 14px;
}

.month ul li {
  font-size: 14px;
  text-transform: uppercase;
  letter-spacing: 0px;
  line-height: 42px;
}

.choose-month {
  text-align: center;
}

.arrow {
  padding: 0 12px;
}

.left {
  float: left;
}

.right {
  float: right;
}

.arrow:hover {
  color: #457bc7;
  cursor: pointer;
}

.month ul li.unActive:hover {
  cursor: not-allowed;
}

.weekdays {
  margin: 0;
  padding: 0;
  display: block;
  overflow: hidden;
}

.weekdays li {
  display: block;
  width: 14.2%;
  float: left;
  text-align: center;
  margin-top: 6px;
  margin-bottom: 8px;
  line-height: 24px;
}

.days {
  padding: 0;
  margin: 0;
  display: block;
}

.days li {
  list-style-type: none;
  display: inline-block;
  width: 14.2%;
  text-align: center;
  padding: 6px;
  font-size: 14px;
  color: #48576a;
  box-sizing: border-box;
  line-height: 24px;
}

.days li span {
  display: block;
}

.days li.other-month,
.days li.unDate {
  padding: 6px;
  color: #bfcbd9;
}

.days li:hover {
  cursor: pointer;
  font-weight: 600;
}

.days li.unDate {
  cursor: not-allowed;
}

.days li.other-month:hover {
  color: #457bc7;
}

.days li.unActive:hover {
  cursor: not-allowed;
}

.days li.monthActive span {
  outline: 1px solid #43a800;
}
.stop {
  cursor: not-allowed !important;
}
.stop span {
  outline: none !important;
}
.days li.otherActive {
  background-color: #eee;
}
.days li.active span {
  background-color: #457bc7;
  outline-color: #457bc7;
  color: #fff;
}

.days li span.curActive {
  outline: 1px solid #457bc7;
}

.days li.active:hover {
  color: #fff;
}
</style>
