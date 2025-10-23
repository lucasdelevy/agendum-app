<template>
  <div v-if="isOpen" class="modal-overlay" @click="closeModal">
    <div class="modal-content" @click.stop>
      <h2>Create Account</h2>
      
      <form @submit.prevent="handleSubmit" class="signup-form">
        <div class="form-row">
          <div class="form-group">
            <input v-model="firstName" type="text" placeholder="First Name" required class="form-input" :disabled="loading">
          </div>
          <div class="form-group">
            <input v-model="lastName" type="text" placeholder="Last Name" required class="form-input" :disabled="loading">
          </div>
        </div>
        
        <div class="form-group">
          <input v-model="username" type="text" placeholder="Username" required class="form-input" :disabled="loading">
        </div>
        
        <div class="form-group">
          <input v-model="email" type="email" placeholder="Email" required class="form-input" :disabled="loading">
        </div>
        
        <div class="form-group">
          <input v-model="password" type="password" placeholder="Password" required class="form-input" :disabled="loading">
        </div>
        
        <div class="form-group">
          <select v-model="userType" class="form-input" :disabled="loading">
            <option value="user">User</option>
            <option value="admin">Admin</option>
          </select>
        </div>
        
        <div v-if="error" class="error-message">{{ error }}</div>
        
        <div class="modal-actions">
          <button type="button" @click="closeModal" class="cancel-button" :disabled="loading">Cancel</button>
          <button type="submit" class="signup-button" :disabled="loading">
            {{ loading ? 'Creating...' : 'Create Account' }}
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import { API_BASE_URL } from '../config/api.js'

export default {
  name: 'SignUpModal',
  props: {
    isOpen: Boolean
  },
  data() {
    return {
      username: '',
      email: '',
      password: '',
      firstName: '',
      lastName: '',
      userType: 'user',
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
      this.username = ''
      this.email = ''
      this.password = ''
      this.firstName = ''
      this.lastName = ''
      this.userType = 'user'
      this.error = ''
    },
    async handleSubmit() {
      this.loading = true
      this.error = ''
      
      try {
        const response = await fetch(`${API_BASE_URL}/users/create`, {
          method: 'POST',
          mode: 'cors',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            username: this.username,
            email: this.email,
            password: this.password,
            firstName: this.firstName,
            lastName: this.lastName,
            userType: this.userType
          })
        })
        
        const data = await response.json()
        
        if (response.ok) {
          this.$emit('user-created', data)
          this.closeModal()
        } else {
          this.error = data.message || 'Failed to create account'
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

.signup-form {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 15px;
}

.form-group {
  display: flex;
  flex-direction: column;
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
  margin-top: 10px;
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

.signup-button {
  background: #667eea;
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
}

.signup-button:hover {
  background: #5a6fd8;
}

.signup-button:disabled,
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