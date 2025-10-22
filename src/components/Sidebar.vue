<template>
  <div class="sidebar-overlay" v-if="isOpen" @click="closeSidebar"></div>
  <aside class="sidebar" :class="{ 'sidebar-open': isOpen }">
    <nav class="sidebar-nav">
      <a href="#" class="nav-item">
        <span class="nav-icon">👤</span>
        My Account
      </a>
      <a href="#" class="nav-item">
        <span class="nav-icon">📋</span>
        My Tasks
      </a>
      <a href="#" class="nav-item">
        <span class="nav-icon">👥</span>
        My Groups
      </a>
      <a href="#" class="nav-item">
        <span class="nav-icon">📅</span>
        My Calendar
      </a>
    </nav>
  </aside>
</template>

<script>
export default {
  name: 'Sidebar',
  props: {
    isOpen: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    closeSidebar() {
      this.$emit('close')
    }
  }
}
</script>

<style scoped>
.sidebar-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 999;
}

.sidebar {
  position: fixed;
  top: 60px;
  left: -280px;
  width: 280px;
  height: calc(100vh - 60px);
  background: white;
  box-shadow: 2px 0 8px rgba(0,0,0,0.1);
  transition: left 0.3s ease;
  z-index: 1000;
}

.sidebar-open {
  left: 0;
}

.sidebar-nav {
  padding: 20px 0;
}

.nav-item {
  display: flex;
  align-items: center;
  padding: 15px 20px;
  color: #333;
  text-decoration: none;
  transition: background 0.2s;
  gap: 15px;
}

.nav-item:hover {
  background: #f5f5f5;
}

.nav-icon {
  font-size: 18px;
  width: 24px;
}
</style>