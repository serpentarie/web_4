<script setup>
import axios from "axios";
import Swal from "sweetalert2";
import {ref} from "vue";
import CryptoJS from "crypto-js";

const username = ref('');
const password = ref('');

const handleRegister = async () => {
  try {
    const hashedPassword = CryptoJS.SHA256(password.value).toString(CryptoJS.enc.Base64);

    const response = await axios.post('/api/register', {
      username: username.value,
      password: hashedPassword,
    });

    if (response.status === 200) {
      Swal.fire({
        title: 'Регистрация успешна!:)',
        text: 'Добро пожаловать!',
        icon: 'success',
        confirmButtonText: 'Перейти к странице входа',
      }).then(() => {
        window.location.href = '/login';
      });
    } else {
      Swal.fire({
        title: 'Ошибка регистрации!:(',
        text: 'Введены неправильные или повторяющиеся данные',
        icon: 'error',
        confirmButtonText: 'Попробовать снова',
      });
    }
  } catch (error) {
    if (error.response?.status === 409) {
      Swal.fire({
        title: 'Ошибка регистрации!',
        text: 'Такой пользователь уже зарегистрирован',
        icon: 'error',
        confirmButtonText: 'Попробовать снова',
      });
    } else {
      console.error('Registration error:', error);
      Swal.fire({
        title: 'Ошибка!:(',
        text: 'Во время выполнения возникла ошибка на сервере. Попробуйте позже',
        icon: 'error',
        confirmButtonText: 'OK',
      });
    }
  }
}
</script>

<template>
  <div class="registration-container">
    <h1>Регистрация</h1>
    <form @submit.prevent="handleRegister">
      <div class="form-group">
        <label for="username">Имя пользователя:</label>
        <input id="username" v-model="username" type="text" placeholder="Введите логин" required/>
      </div>
      <div class="form-group">
        <label for="password">Пароль:</label>
        <input id="password" v-model="password" type="password" placeholder="Введите пароль" required/>
      </div>
      <button type="submit">Зарегистрироваться</button>
    </form>
    <p>Уже есть аккаунт?
      <router-link to="/login">Войти</router-link>
    </p>
  </div>
</template>

<style scoped>
.registration-container {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background: linear-gradient(rgba(255, 255, 255, 0.4), rgba(255, 255, 255, 0.4)), url("2.jpg") no-repeat center center;
  background-size: cover;
  border-radius: 20px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease-in-out;
}

.registration-container:hover {
  transform: scale(1.02);
}


h1 {
  font-size: 2.5rem;
  text-align: center;
  color: #333;
}

.form-group {
  margin-bottom: 20px;
}

input {
  width: 100%;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 10px;
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

@media (min-width: 1110px) {
  .registration-container {
    max-width: 600px;
  }
}

@media (max-width: 1109px) and (min-width: 765px) {
  .registration-container {
    max-width: 90%;
  }
}

@media (max-width: 764px) {
  .registration-container {
    max-width: 100%;
    padding: 10px;
  }
}
</style>
