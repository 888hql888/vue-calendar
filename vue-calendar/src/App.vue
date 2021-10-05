<template>
  <div id="app">
    <!-- <Calendar :activeDay="activeDay" :monthDay="monthDay" /> -->
    <Calendar2  />
    
  </div>
</template>

<script>
import Calendar2 from './components/Calendar2.vue'
// import Calendar from './components/Calendar.vue'

export default {
  name: 'App',
  components: {
    // Calendar,
    Calendar2
  },
  data(){
    return{
      activeDay:new Date('2021-10-05'),
      monthDay:['2021-10-08','2021-10-09','2021-10-10']
    }
  }
}
</script>

<style>
*{
  padding: 0;
  margin: 0;
}
ul li {
  list-style: none;
}
</style>
