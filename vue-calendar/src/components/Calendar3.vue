<template>
  <div class="calendar-main">
    <div class="main-view">
      <div class="header">
        <span @click="preMonth"> ← </span>
        <span class="yearMonth"> 
          <span>{{currentYear}}</span>
          <span style="padding:0 6px">年</span>
          <span>{{currentMonth}}</span>
          <span style="padding:0 6px">月</span>
        </span>
        <span @click="nextMonth"> → </span>
      </div>
      <ul class="weekdays">
                <li v-for="(item,index) in weekDays" :key="index" >{{item}}</li>
      </ul>
      <ul class="numdays">
        <li v-for="(item,i) in daysArr" :key="i">
          {{item.getDate()}}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props:{

  },
  created(){
    this.initDaysData()
  },
  data(){
    const date = new Date()
    return{
        currentYear:date.getFullYear(),
        currentMonth:date.getMonth()+1,
        daysArr:[], //存储日期的数组
      weekDays:['日','一','二','三','四','五','六']

    }
  },
  methods:{
    initDaysData(cur){
      this.daysArr = [] //先清空数据
      //以42个小格为准
      if(cur){
        let date = new Date(cur)
        this.currentYear = date.getFullYear()
        this.currentMonth = date.getMonth()+1
      }
      //计算是周几
      const {currentYear,currentMonth} = this
      let firstDate = new Date(this.formData(currentYear,currentMonth,1))
      let firstWeekDay = firstDate.getDay() //周几

      if(firstWeekDay===0) firstWeekDay = 7
      //计算前一个月
      for(let i=firstWeekDay;i>0;i--){
        let d = new Date(firstDate)
        d.setDate(d.getDate()-i)
        this.daysArr.push(d)
      }
      //计算剩下的天数
      for(let i=0;i < 42 - firstWeekDay;i++){
        let d = new Date(firstDate)
        d.setDate(d.getDate()+i)
        this.daysArr.push(d)
      }


    },
    formData(y,m,d){
      m = m < 10 ? '0'+ m : m
      d = d < 10 ? '0'+ d : d
      return `${y}/${m}/${d}`
    },
    preMonth(){
      const {currentYear,currentMonth} = this
      let pre = new Date(this.formData(currentYear,currentMonth,1))
      pre.setDate(0)
      this.initDaysData(pre)
    },
    nextMonth(){
      const {currentYear,currentMonth} = this
      let nex = new Date(this.formData(currentYear,currentMonth,1))
      nex.setDate(35)
      this.initDaysData(nex)
      console.log('111')
    }
  }
};
</script>

<style lang="less" scoped>
.calendar-main {
  display: flex;
  align-content: center;
  justify-content: center;
  .main-view{
  padding: 20px 0;
  width: 30vw;
  height: 45vh;
  border: 1px solid #e0e0e0;
  margin: 24px 0;
  .header, .weekdays, .numdays {
    padding: 0 20px;
  }
    .header{
      display: flex;
      padding-bottom: 12px;
      justify-content: space-between;
      border-bottom: 1px solid #e0e0e0;
    }
    .weekdays ,.numdays{
      display: flex;
    }
    .numdays{
      flex-wrap: wrap;
    }
    .weekdays>li , .numdays>li {
        width: 14.2%;
        padding: 10px 0;
        height: 20px;
        line-height: 20px;
        text-align: center;
    }
  }
}
</style>