<template>
  <div id="app">
    <AppHeader @toggle-sidebar="toggleSidebar" />
    <Sidebar :isOpen="sidebarOpen" @close="closeSidebar" />
    <main class="main-content">
      <WeeklyCalendar />
    </main>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue'
import Sidebar from './components/Sidebar.vue'
import WeeklyCalendar from './components/WeeklyCalendar.vue'

export default {
  name: 'App',
  components: {
    AppHeader,
    Sidebar,
    WeeklyCalendar
  },
  data() {
    return {
      sidebarOpen: false
    }
  },
  methods: {
    toggleSidebar() {
      this.sidebarOpen = !this.sidebarOpen
    },
    closeSidebar() {
      this.sidebarOpen = false
    }
  }
}
</script>

<style>
body {
  margin: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: #f8f9fa;
}

#app {
  min-height: 100vh;
}

.main-content {
  padding: 20px;
  margin-top: 60px;
}
</style>