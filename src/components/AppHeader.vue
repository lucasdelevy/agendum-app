<template>
  <header class="app-header">
    <button @click="toggleSidebar" class="menu-button">
      <span></span>
      <span></span>
      <span></span>
    </button>
    <h1>Agendum</h1>
    <button @click="logout" class="logout-button">Logout</button>
  </header>
</template>

<script>
export default {
  name: 'AppHeader',
  methods: {
    toggleSidebar() {
      this.$emit('toggle-sidebar')
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