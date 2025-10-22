<template>
  <header class="app-header">
    <button @click="toggleSidebar" class="menu-button">
      <span></span>
      <span></span>
      <span></span>
    </button>
    <h1>Agendum</h1>
    <div class="header-actions">
      <button @click="toggleDarkMode" class="dark-mode-button">{{ darkMode ? '☀️' : '🌙' }}</button>
      <button @click="logout" class="logout-button">Logout</button>
    </div>
  </header>
</template>

<script>
export default {
  name: 'AppHeader',
  props: {
    darkMode: Boolean
  },
  methods: {
    toggleSidebar() {
      this.$emit('toggle-sidebar')
    },
    toggleDarkMode() {
      this.$emit('toggle-dark-mode')
    },
    logout() {
      this.$emit('logout')
    }
  }
}
</script>

<style scoped>
.app-header {
  background: #2c3e50;
  color: white;
  padding: 0 20px;
  height: 60px;
  display: flex;
  align-items: center;
  gap: 20px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.menu-button {
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.menu-button span {
  width: 24px;
  height: 3px;
  background: white;
  border-radius: 2px;
  transition: 0.3s;
}

h1 {
  margin: 0;
  font-size: 24px;
  font-weight: 600;
  flex: 1;
}

.header-actions {
  display: flex;
  gap: 10px;
  align-items: center;
}

.dark-mode-button {
  background: none;
  border: none;
  font-size: 20px;
  cursor: pointer;
  padding: 8px;
  border-radius: 4px;
  transition: background 0.2s;
}

.dark-mode-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

.logout-button {
  background: #dc3545;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.logout-button:hover {
  background: #c82333;
}
</style>