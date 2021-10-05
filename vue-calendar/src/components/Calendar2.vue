<template>
  <div class="calendar-main">
    <div class="main-view">
      <div class="header">
        <span> ← </span>
        <span class="yearMonth"> 
          <span>2021</span>
          <span style="padding:0 6px">年</span>
          <span>10</span>
          <span style="padding:0 6px">月</span>
        </span>
        <span> → </span>
      </div>
      <ul class="weekdays">
        <li>日</li>
        <li>一</li>
        <li>二</li>
        <li>三</li>
        <li>四</li>
        <li>五</li>
        <li>六</li>
      </ul>
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
    }
  },
  methods:{
    initDaysData(curDate){
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
    .weekdays>li , .numdays>li {
        width: 20px;
        padding: 10px;
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