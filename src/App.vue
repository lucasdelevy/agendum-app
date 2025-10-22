<template>
  <div id="app">
    <WeeklyCalendar />
  </div>
</template>

<script>
import WeeklyCalendar from './components/WeeklyCalendar.vue'

export default {
  name: 'App',
  components: {
    WeeklyCalendar
  }
}
</script>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
}

#app {
  padding: 20px;
}
</style>