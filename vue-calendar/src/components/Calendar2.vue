<template>
  <div class="calendar-main">
    <div class="main-view">
      <!-- 年月 -->
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
      <!-- 周几 -->
      <ul class="weekdays">
        <li v-for="(item,index) in weekDays" :key="index" >{{item}}</li>
      </ul>
      <!-- 几号 -->
      <ul class="numdays">
        <li v-for="(day,index) in daysArr" :key="index">
          {{day.getDate()}}
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
      daysArr:[], //存储日历上的numdays 号数
      currentYear:date.getFullYear(),
      currentMonth:date.getMonth()+1,
      weekDays:['日','一','二','三','四','五','六']
    }
  },
  methods:{
    initDaysData(curDate){
      this.daysArr = []
      //以42个小格为准
      let date = new Date()
      if(curDate){
        date = new Date(curDate)
        this.currentYear = date.getFullYear()
        this.currentMonth = date.getMonth()+1
        this.currentWeek = date.getDay()
      }
      if(this.currentWeek==0) this.currentWeek = 7
      let firstDate = new Date(this.formDate(this.currentYear,this.currentMonth,1))
      let currentWeek = firstDate.getDay()
      for(let i = currentWeek;i>0;i--){
        let d = new Date()
        d.setDate(firstDate.getDate()-i)
        this.daysArr.push(d)
      }
      for(let i = 0;i<42-currentWeek;i++){
        let d = new Date()
        d.setDate(firstDate.getDate()+i)
        this.daysArr.push(d)
      }

      
    },
    formDate(year,month,day){
     if(!year || !month || !day) return
     month = month < 10 ? '0'+month : month
     day = day < 10 ? '0'+day : day
     return `${year}/${month}/${day}`
    },
    preMonth(){
      const {currentYear,currentMonth} = this
      let preDate = new Date(this.formDate(currentYear,currentMonth,1))
      preDate.setDate(0) //设置上个月
      this.initDaysData(preDate)
    },
    nextMonth(){
      const {currentYear,currentMonth} = this
      let nextDate = new Date(this.formDate(currentYear,currentMonth,1))
      nextDate.setDate(35) //设置下个月
      this.initDaysData(nextDate)
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
    .weekdays , .numdays{
      display: flex;
    }
    .weekdays>li , .numdays>li {
        width: 14.2%;
        padding: 10px 0;
        height: 20px;
        line-height: 20px;
        text-align: center;
    }
    .numdays {
      flex-wrap: wrap;
    }
  }
}
</style>