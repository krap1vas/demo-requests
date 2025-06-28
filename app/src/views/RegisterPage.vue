<template>
    <div class="classs">
        <h1>Регистрация</h1>
        <input type="text" placeholder="Введите ваше ФИО" v-model="fio">
        <input type="text" placeholder="Введите почту" v-model="email">
        <input type="text" placeholder="Введите телефон" v-model="phone">
        <input type="text" placeholder="Введите логин" v-model="username">
        <input type="password" placeholder="Введие пароль" v-model="password">
        <button @click="register">Зарегестироваться</button>
        <button @click="router.push('/auth')">На авторизацию</button>
    </div>
</template>

<script setup lang="ts">
import router from '@/router';
import axios from 'axios';
import { ref } from 'vue';

const username = ref('')
const password = ref('')
const fio = ref('')
const email = ref('')
const phone = ref()

async function register() {

    // if (username.value.length && password.value.length < 6) {
    //     alert('Логин или пароль меньше 6 символов'); 
    //     return;
    // }
    // if (!email.value.includes('@')) {
    //     alert ('Неправильный формат почты. Форма должна содержать @'); 
    //     return;
    // }

    // if (!/^8 \(\d{3}\) \d{3}-\d{2}-\d{2}$/.test(phone.value)){
    //     alert('Неверный формат номера телефона. Нужный формат: x (xxx) xxx-xx-xx');
    //     return
    // }

    const data = {
        fio: fio.value,
        email: email.value,
        phone: phone.value,
        username: username.value,
        password: password.value,
    }

    await axios.post('http://localhost:1488/register', data)
    const token = await axios.post('http://localhost:1488/login', data)
    localStorage.setItem('token', token.data.access_token)
    localStorage.setItem('username', username.value)
    localStorage.setItem('fio', fio.value)
    router.push('/main')
}
</script>

<style scoped>
.classs {
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin: 5px;
    padding: 5px;
    align-items: center;
}
</style>
