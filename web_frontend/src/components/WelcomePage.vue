<script setup>
import { ref } from 'vue';
import axios from 'axios';
import Swal from 'sweetalert2';
import CryptoJS from "crypto-js";

const username = ref('');
const password = ref('');

const handleLogin = async () => {
  try {
    const hashedPassword = CryptoJS.SHA256(password.value).toString(CryptoJS.enc.Base64);

    const response = await axios.post('/api/login', {
      username: username.value,
      password: hashedPassword,
    });

    if (response.status === 200) {
      Swal.fire({
        title: 'Вход успешен!',
        text: 'Добро пожаловать:)',
        icon: 'success',
        confirmButtonText: 'Перейти к главной странице',
      }).then(() => {
        localStorage.setItem('username', username.value);
        localStorage.setItem('password', hashedPassword);
        window.location.href = '/main';
      });
    } else {
      Swal.fire({
        title: 'Ошибка входа!:(',
        text: 'Неправильный пароль',
        icon: 'error',
        confirmButtonText: 'Попробовать снова',
      });
    }
  } catch (error) {
    if (error.response?.status === 401) {
      Swal.fire({
        title: 'Ошибка входа!:(',
        text: 'Такого пользователя не существует',
        icon: 'error',
        confirmButtonText: 'Попробовать зарегистрироваться',
      });
    } else {
      console.error('Login error:', error);
      Swal.fire({
        title: 'Ошибка!:(',
        text: 'Во время выполнения возникла ошибка на сервере. Попробуйте позже',
        icon: 'error',
        confirmButtonText: 'OK',
      });
    }
  }
};
</script>

<template>
  <div class="login-container">
    <h2>Лабораторная 4</h2>
    <h3>Группа: P3209</h3>
    <h3>Вариант: 124564</h3>

    <h3>Пожалуйста, войдите</h3>

    <form @submit.prevent="handleLogin">
      <div class="form-group">
        <label for="username">Имя пользователя:</label>
        <input id="username" v-model="username" type="text" placeholder="Введите логин" required/>
      </div>
      <div class="form-group">
        <label for="password">Пароль:</label>
        <input id="password" v-model="password" type="password" placeholder="Введите пароль" required/>
      </div>
      <button type="submit">Войти</button>
    </form>

    <p>Нет аккаунта?
      <router-link to="/register">Зарегистрироваться</router-link>
    </p>
  </div>
</template>

<style scoped>

.login-container {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background: linear-gradient(rgba(255, 255, 255, 0.7), rgba(255, 255, 255, 0.7)), url("1.jpg") no-repeat center center;
  background-size: cover;
  border-radius: 20px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease-in-out;
}

.login-container:hover {
  transform: scale(1.02);
}

h1, h2 {
  font-family: 'Poppins', sans-serif;
  text-align: center;
  color: #333;
}

h1 {
  font-size: 2.5rem;
}

h2 {
  font-size: 1.8rem;
}

h3, p {
  text-align: center;
  color: #555;
}

.form-group {
  margin-bottom: 20px;
}

label {
  font-weight: bold;
}

input {
  width: 100%;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 10px;
  box-shadow: inset 0 2px 5px rgba(0,0,0,0.1);
}

button {
  width: 100%;
  padding: 15px;
  background: linear-gradient(90deg, #ff416c, #ff4b2b);
  color: white;
  border: none;
  border-radius: 10px;
  font-size: 1.2rem;
  cursor: pointer;
  transition: background 0.3s ease;
}

button:hover {
  background: linear-gradient(90deg, #000000, #715f63);
}

router-link {
  color: #ff0000;
}

router-link:hover {
  text-decoration: none;
}

@media (min-width: 1110px) {
  .login-container {
    max-width: 600px;
  }
}

@media (max-width: 1109px) and (min-width: 765px) {
  .login-container {
    max-width: 90%;
  }
}

@media (max-width: 764px) {
  .login-container {
    max-width: 100%;
    padding: 10px;
  }
  h1 {
    font-size: 2rem;
  }
  h2 {
    font-size: 1.5rem;
  }
}
</style>
