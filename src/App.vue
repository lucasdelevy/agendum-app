<template>
  <div id="app" :class="{ 'dark-mode': darkMode }">
    <LoginPage v-if="!isLoggedIn" @login="handleLogin" />
    <div v-else>
      <AppHeader :darkMode="darkMode" @toggle-sidebar="toggleSidebar" @create-team="openCreateTeamModal" @toggle-dark-mode="toggleDarkMode" @logout="handleLogout" />
      <TeamsList ref="teamsList" :userToken="user?.token" @team-selected="handleTeamSelected" />
      <Sidebar :isOpen="sidebarOpen" @close="closeSidebar" />
      <main class="main-content">
        <WeeklyCalendar />
      </main>
      <CreateTeamModal :isOpen="showCreateTeamModal" :userToken="user?.token" @close="closeCreateTeamModal" @team-created="handleTeamCreated" />
    </div>
  </div>
</template>

<script>
import LoginPage from './components/LoginPage.vue'
import AppHeader from './components/AppHeader.vue'
import TeamsList from './components/TeamsList.vue'
import Sidebar from './components/Sidebar.vue'
import WeeklyCalendar from './components/WeeklyCalendar.vue'
import CreateTeamModal from './components/CreateTeamModal.vue'

export default {
  name: 'App',
  components: {
    LoginPage,
    AppHeader,
    TeamsList,
    Sidebar,
    WeeklyCalendar,
    CreateTeamModal
  },
  data() {
    return {
      isLoggedIn: false,
      user: null,
      sidebarOpen: false,
      darkMode: false,
      showCreateTeamModal: false
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
    },
    toggleDarkMode() {
      this.darkMode = !this.darkMode
      localStorage.setItem('agendum_dark_mode', this.darkMode)
    },
    openCreateTeamModal() {
      this.showCreateTeamModal = true
    },
    closeCreateTeamModal() {
      this.showCreateTeamModal = false
    },
    handleTeamCreated(teamData) {
      console.log('Team created:', teamData)
      // Refresh teams list after creation
      this.$refs.teamsList.fetchTeams()
    },
    handleTeamSelected(team) {
      console.log('Team selected:', team)
      // Handle team selection (e.g., filter calendar by team)
    }
  },
  mounted() {
    const savedUser = localStorage.getItem('agendum_user')
    if (savedUser) {
      this.user = JSON.parse(savedUser)
      this.isLoggedIn = true
    }
    
    const savedDarkMode = localStorage.getItem('agendum_dark_mode')
    if (savedDarkMode) {
      this.darkMode = savedDarkMode === 'true'
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

.dark-mode {
  background: #1a1a1a;
  color: #ffffff;
}

.dark-mode .app-header {
  background: #333;
}

.dark-mode .calendar-container {
  background: #2d2d2d;
  border-color: #444;
}

.dark-mode .day-header {
  background: #2d2d2d;
  border-color: #444;
}

.dark-mode .current-day-header {
  background: #1e3a8a;
}

.dark-mode .hour-cell {
  border-color: #444;
}

.dark-mode .current-day-column {
  background: #1e293b;
}
</style>