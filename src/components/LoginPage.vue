<template>
  <div class="login-container">
    <div class="login-card">
      <h1 class="login-title">Agendum</h1>
      <p class="login-subtitle">Sign in to your account</p>
      
      <form @submit.prevent="handleLogin" class="login-form">
        <div class="form-group">
          <input 
            v-model="email" 
            type="email" 
            placeholder="Email" 
            required 
            class="form-input"
            :disabled="loading"
          >
        </div>
        
        <div class="form-group">
          <input 
            v-model="password" 
            :type="showPassword ? 'text' : 'password'" 
            placeholder="Password" 
            required 
            class="form-input"
            :disabled="loading"
          >
        </div>
        
        <div class="checkbox-group">
          <input 
            v-model="showPassword" 
            type="checkbox" 
            id="showPassword"
          >
          <label for="showPassword">Show password</label>
        </div>
        
        <div v-if="error" class="error-message">{{ error }}</div>
        
        <button type="submit" class="login-button" :disabled="loading">
          {{ loading ? 'Signing In...' : 'Sign In' }}
        </button>
        
        <button type="button" @click="openSignUpModal" class="signup-link-button">
          Don't have an account? Sign Up
        </button>
      </form>
    </div>
    
    <SignUpModal :isOpen="showSignUpModal" @close="closeSignUpModal" @user-created="handleUserCreated" />
  </div>
</template>

<script>
import SignUpModal from './SignUpModal.vue'
import { API_BASE_URL } from '../config/api.js'

export default {
  name: 'LoginPage',
  components: {
    SignUpModal
  },
  data() {
    return {
      email: '',
      password: '',
      showPassword: false,
      loading: false,
      error: '',
      showSignUpModal: false
    }
  },
  methods: {
    async handleLogin() {
      this.loading = true
      this.error = ''
      
      try {
        const response = await fetch(`${API_BASE_URL}/auth/login`, {
          method: 'POST',
          mode: 'cors',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            email: this.email,
            password: this.password
          })
        })
        
        const data = await response.json()
        
        if (response.ok) {
          this.$emit('login', data)
        } else {
          this.error = data.message || 'Login failed'
        }
      } catch (err) {
        this.error = 'Network error. Please try again.'
      } finally {
        this.loading = false
      }
    },
    openSignUpModal() {
      this.showSignUpModal = true
    },
    closeSignUpModal() {
      this.showSignUpModal = false
    },
    handleUserCreated(userData) {
      console.log('User created:', userData)
      // Optionally auto-login or show success message
    }
  }
}
</script>

<style scoped>
.login-container {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.login-card {
  background: white;
  padding: 40px;
  border-radius: 12px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
  width: 100%;
  max-width: 400px;
}

.login-title {
  text-align: center;
  margin: 0 0 10px 0;
  font-size: 32px;
  font-weight: 600;
  color: #2c3e50;
}

.login-subtitle {
  text-align: center;
  margin: 0 0 30px 0;
  color: #666;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
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

.login-button {
  background: #667eea;
  color: white;
  border: none;
  padding: 12px;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s;
}

.login-button:hover {
  background: #5a6fd8;
}

.login-button:disabled {
  background: #ccc;
  cursor: not-allowed;
}

.form-input:disabled {
  background: #f5f5f5;
  cursor: not-allowed;
}

.checkbox-group {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 14px;
}

.checkbox-group input[type="checkbox"] {
  margin: 0;
}

.checkbox-group label {
  cursor: pointer;
  color: #666;
}

.signup-link-button {
  background: none;
  border: none;
  color: #667eea;
  font-size: 14px;
  cursor: pointer;
  text-decoration: underline;
  margin-top: 10px;
}

.signup-link-button:hover {
  color: #5a6fd8;
}

.error-message {
  color: #dc3545;
  font-size: 14px;
  text-align: center;
  margin: -10px 0 10px 0;
}
</style>