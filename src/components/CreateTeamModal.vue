<template>
  <div v-if="isOpen" class="modal-overlay" @click="closeModal">
    <div class="modal-content" @click.stop>
      <h2>Create New Team</h2>
      
      <form @submit.prevent="handleSubmit" class="team-form">
        <div class="form-group">
          <label>Team Name</label>
          <input 
            v-model="teamName" 
            type="text" 
            placeholder="Enter team name" 
            required 
            class="form-input"
            :disabled="loading"
          >
        </div>
        
        <div class="form-group">
          <label>Admins (comma-separated usernames)</label>
          <input 
            v-model="adminsInput" 
            type="text" 
            placeholder="john_doe, jane_smith" 
            class="form-input"
            :disabled="loading"
          >
        </div>
        
        <div class="form-group">
          <label>Members (comma-separated usernames)</label>
          <input 
            v-model="membersInput" 
            type="text" 
            placeholder="alice_jones, bob_wilson" 
            class="form-input"
            :disabled="loading"
          >
        </div>
        
        <div v-if="error" class="error-message">{{ error }}</div>
        
        <div class="modal-actions">
          <button type="button" @click="closeModal" class="cancel-button" :disabled="loading">Cancel</button>
          <button type="submit" class="create-button" :disabled="loading">
            {{ loading ? 'Creating...' : 'Create Team' }}
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import { API_BASE_URL } from '../config/api.js'

export default {
  name: 'CreateTeamModal',
  props: {
    isOpen: Boolean,
    userToken: String
  },
  data() {
    return {
      teamName: '',
      adminsInput: '',
      membersInput: '',
      loading: false,
      error: ''
    }
  },
  methods: {
    closeModal() {
      this.$emit('close')
      this.resetForm()
    },
    resetForm() {
      this.teamName = ''
      this.adminsInput = ''
      this.membersInput = ''
      this.error = ''
    },
    async handleSubmit() {
      this.loading = true
      this.error = ''
      
      try {
        const admins = this.adminsInput.split(',').map(s => s.trim()).filter(s => s)
        const members = this.membersInput.split(',').map(s => s.trim()).filter(s => s)
        
        const response = await fetch(`${API_BASE_URL}/teams/create`, {
          method: 'POST',
          mode: 'cors',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${this.userToken}`
          },
          body: JSON.stringify({
            name: this.teamName,
            admins: admins,
            members: members
          })
        })
        
        const data = await response.json()
        
        if (response.ok) {
          this.$emit('team-created', data)
          this.closeModal()
        } else {
          this.error = data.message || 'Failed to create team'
        }
      } catch (err) {
        this.error = 'Network error. Please try again.'
      } finally {
        this.loading = false
      }
    }
  }
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  padding: 30px;
  border-radius: 12px;
  width: 100%;
  max-width: 500px;
  max-height: 90vh;
  overflow-y: auto;
}

h2 {
  margin: 0 0 20px 0;
  color: #2c3e50;
}

.team-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

label {
  font-weight: 600;
  color: #555;
}

.form-input {
  padding: 12px 16px;
  border: 2px solid #e1e5e9;
  border-radius: 8px;
  font-size: 16px;
  transition: border-color 0.2s;
}

.form-input:focus {
  outline: none;
  border-color: #667eea;
}

.form-input:disabled {
  background: #f5f5f5;
  cursor: not-allowed;
}

.modal-actions {
  display: flex;
  gap: 10px;
  justify-content: flex-end;
}

.cancel-button {
  background: #6c757d;
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
}

.cancel-button:hover {
  background: #5a6268;
}

.create-button {
  background: #28a745;
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
}

.create-button:hover {
  background: #218838;
}

.create-button:disabled,
.cancel-button:disabled {
  background: #ccc;
  cursor: not-allowed;
}

.error-message {
  color: #dc3545;
  font-size: 14px;
  text-align: center;
}
</style>