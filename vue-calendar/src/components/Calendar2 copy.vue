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
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>6</li>
        <li>7</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props:{

  },
  data(){
    return{
      
    }
  },
  methods:{
    initDaysData(){

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
      flex: 1;
        padding: 10px;
        height: 20px;
        line-height: 20px;
        text-align: center;
    }
  }
}
</style>