<template>
  <div class="teams-container">
    <div class="teams-list">
      <button 
        v-for="team in teams" 
        :key="team.team_id" 
        class="team-button"
        :style="{ backgroundColor: getTeamColor(team.team_id) }"
        @click="selectTeam(team)"
      >
        {{ team.name }}
      </button>
    </div>
  </div>
</template>

<script>
import { API_BASE_URL } from '../config/api.js'

export default {
  name: 'TeamsList',
  props: {
    userToken: String
  },
  data() {
    return {
      teams: []
    }
  },
  methods: {
    async fetchTeams() {
      if (!this.userToken) return
      
      try {
        const response = await fetch(`${API_BASE_URL}/teams/list`, {
          method: 'GET',
          mode: 'cors',
          headers: {
            'Authorization': `Bearer ${this.userToken}`
          }
        })
        
        if (response.ok) {
          this.teams = await response.json()
        }
      } catch (err) {
        console.error('Failed to fetch teams:', err)
      }
    },
    getTeamColor(teamId) {
      const colors = [
        '#FF6B6B', '#4ECDC4', '#45B7D1', '#96CEB4', '#FFEAA7',
        '#DDA0DD', '#98D8C8', '#F7DC6F', '#BB8FCE', '#85C1E9'
      ]
      const hash = teamId.split('').reduce((a, b) => {
        a = ((a << 5) - a) + b.charCodeAt(0)
        return a & a
      }, 0)
      return colors[Math.abs(hash) % colors.length]
    },
    selectTeam(team) {
      this.$emit('team-selected', team)
    }
  },
  mounted() {
    this.fetchTeams()
  },
  watch: {
    userToken() {
      this.fetchTeams()
    }
  }
}
</script>

<style scoped>
.teams-container {
  padding: 15px 20px;
}

.teams-list {
  display: flex;
  gap: 12px;
  overflow-x: auto;
  padding: 5px 0;
  justify-content: center;
}

.team-button {
  min-width: 120px;
  height: 50px;
  border: none;
  border-radius: 12px;
  color: white;
  font-weight: 600;
  font-size: 14px;
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
  white-space: nowrap;
  padding: 0 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.team-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.teams-list::-webkit-scrollbar {
  height: 6px;
}

.teams-list::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 3px;
}

.teams-list::-webkit-scrollbar-thumb {
  background: #c1c1c1;
  border-radius: 3px;
}
</style>