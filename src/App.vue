<template>
  <div id="app">
    <LoginPage v-if="!isLoggedIn" @login="handleLogin" />
    <div v-else>
      <AppHeader @toggle-sidebar="toggleSidebar" @logout="handleLogout" />
      <Sidebar :isOpen="sidebarOpen" @close="closeSidebar" />
      <main class="main-content">
        <WeeklyCalendar />
      </main>
    </div>
  </div>
</template>

<script>
import LoginPage from './components/LoginPage.vue'
import AppHeader from './components/AppHeader.vue'
import Sidebar from './components/Sidebar.vue'
import WeeklyCalendar from './components/WeeklyCalendar.vue'

export default {
  name: 'App',
  components: {
    LoginPage,
    AppHeader,
    Sidebar,
    WeeklyCalendar
  },
  data() {
    return {
      isLoggedIn: false,
      user: null,
      sidebarOpen: false
    }
  },
  methods: {
    handleLogin(userData) {
      this.isLoggedIn = true
      this.user = userData
      localStorage.setItem('agendum_user', JSON.stringify(userData))
    },
    handleLogout() {
      this.isLoggedIn = false
      this.user = null
      this.sidebarOpen = false
      localStorage.removeItem('agendum_user')
    },
    toggleSidebar() {
      this.sidebarOpen = !this.sidebarOpen
    },
    closeSidebar() {
      this.sidebarOpen = false
    }
  },
  mounted() {
    const savedUser = localStorage.getItem('agendum_user')
    if (savedUser) {
      this.user = JSON.parse(savedUser)
      this.isLoggedIn = true
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