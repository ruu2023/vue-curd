<script setup>
import { ref } from 'vue'
import axios from 'axios'

const email = ref('')
const password = ref('')
const error = ref('')

const login = async () => {
  try {
    const response = await axios.post('http://localhost:8000/api/v1/login', {
      email: email.value,
      password: password.value
    })

    // ログイン成功時の処理
    // 例: トークンをローカルストレージに保存
    localStorage.setItem('token', response.data.token)
    console.log('Login successful');
    error.value = ''

    // ここでリダイレクトや状態の更新などを行う
  } catch (e) {
    error.value = 'ログインに失敗しました。メールアドレスとパスワードを確認してください。'
    console.error('Login error:', e)
  }
}
</script>

<template>
  <div class="login-container">
    <h2>ログイン</h2>
    <form @submit.prevent="login" class="login-form">
      <div class="form-group">
        <label for="email">メールアドレス:</label>
        <input
          type="email"
          id="email"
          v-model="email"
          required
          placeholder="example@example.com"
        >
      </div>

      <div class="form-group">
        <label for="password">パスワード:</label>
        <input
          type="password"
          id="password"
          v-model="password"
          required
          placeholder="パスワードを入力"
        >
      </div>

      <div v-if="error" class="error-message">
        {{ error }}
      </div>

      <button type="submit" class="login-button">ログイン</button>
    </form>
  </div>
</template>

<style scoped>
.login-container {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

label {
  font-weight: bold;
}

input {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.login-button {
  padding: 10px;
  background-color: #42b883;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.login-button:hover {
  background-color: #3aa876;
}

.error-message {
  color: red;
  font-size: 14px;
}
</style>
