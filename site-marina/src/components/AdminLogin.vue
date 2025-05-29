<template>
  <div class="admin-login">
    <h1>Вход в админ-панель</h1>
    <div class="login-form">
      <div class="form-group">
        <label for="username">Логин</label>
        <input
          type="text"
          id="username"
          v-model="username"
          placeholder="Введите логин"
          @keyup.enter="login"
        />
      </div>
      <div class="form-group">
        <label for="password">Пароль</label>
        <input
          type="password"
          id="password"
          v-model="password"
          placeholder="Введите пароль"
          @keyup.enter="login"
        />
      </div>
      <button @click="login" :disabled="loading">
        {{ loading ? 'Вход...' : 'Войти' }}
      </button>
      <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'AdminLogin',
  data() {
    return {
      username: '',
      password: '',
      loading: false,
      errorMessage: ''
    };
  },
  methods: {
    async login() {
      if (!this.username || !this.password) {
        this.errorMessage = 'Пожалуйста, заполните все поля';
        return;
      }

      this.loading = true;
      this.errorMessage = '';

      try {
        // Отправляем запрос на бэкенд
        const response = await axios.get('http://localhost:3000/auth');
        if (this.username === response.data.username && this.password === response.data.password) {
        localStorage.setItem('authToken', response.data.token);
        this.$router.push('/admin');
        } else {
        this.errorMessage = 'Неверный логин или пароль';
        }
      } catch (error) {
        this.errorMessage = 'Ошибка при входе. Проверьте логин и пароль.';
        console.error('Login error:', error);
      } finally {
        this.loading = false;
      }
    }
  }
};
</script>

<style scoped>
.admin-login {
  width: 100%;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: #f5f7fa;
  font-family: 'Nekst', sans-serif;
  padding: 2rem;
}

h1 {
  font-family: 'Nekst-SemiBold', sans-serif;
  font-size: clamp(2rem, 3.5vw, 4rem);
  color: #1a202c;
  margin-bottom: 3vh;
}

.login-form {
  width: 100%;
  max-width: 400px;
  background-color: #fff;
  padding: 2rem;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 1.5rem;
}

label {
  display: block;
  font-family: 'Nekst-Medium', sans-serif;
  font-size: clamp(1rem, 1.2vw, 1.2rem);
  color: #4a5568;
  margin-bottom: 0.5rem;
}

input {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid #e0e0e0;
  border-radius: 5px;
  font-family: 'Nekst-Light', sans-serif;
  font-size: clamp(1rem, 1.2vw, 1.2rem);
  color: #1a202c;
  outline: none;
  transition: border-color 0.3s;
}

input:focus {
  border-color: #5552e8;
}

button {
  width: 100%;
  padding: 0.75rem;
  background-color: #5552e8;
  color: #fff;
  border: none;
  border-radius: 5px;
  font-family: 'Nekst-Medium', sans-serif;
  font-size: clamp(1rem, 1.2vw, 1.2rem);
  cursor: pointer;
  transition: background-color 0.3s;
}

button:disabled {
  background-color: #a0a0a0;
  cursor: not-allowed;
}

button:hover:not(:disabled) {
  background-color: #4542c8;
}

.error {
  color: #e53e3e;
  font-family: 'Nekst-Light', sans-serif;
  font-size: clamp(0.9rem, 1vw, 1rem);
  margin-top: 1rem;
  text-align: center;
}

/* Адаптивность для мобильных устройств */
@media (max-width: 768px) {
  .admin-login {
    padding: 1rem;
  }

  h1 {
    font-size: clamp(1.5rem, 5vw, 2rem);
  }

  .login-form {
    padding: 1.5rem;
  }

  input,
  button {
    font-size: clamp(0.9rem, 3vw, 1rem);
  }
}
</style>