<template>
    <div class="classs">
        <h1>Создания заявки</h1>
        <input type="date" placeholder="Выберите дату бронирования" v-model="dataP">
        <input type="time" placeholder="Выберите время для бронирования" v-model="time">
        <input type="text" placeholder="Марка автомобиля" v-model="marka">
        <input type="text" placeholder="Модель автомобиля" v-model="model">
        <input type="text" placeholder="Адрес" v-model="adress">
        <label>
        <input type="checkbox" v-model="rulesChecked" />Я ознакомлен с правилами аренды
        </label>
        <button @click="addRequest">Отправить заявку</button>
    </div>
</template>

<script setup lang="ts">
import router from '@/router';
import axios from 'axios';
import { ref } from 'vue';

const dataP = ref('')
const time = ref('')
const marka = ref('')
const model = ref('')
const rulesChecked = ref(false)
const adress = ref('')

async function addRequest() {
    const config = {
        headers: {
            Authorization: `Bearer ${localStorage.getItem('token')}`
        }
    }

    const data = {
        data: dataP.value,
        time: time.value,
        marka: marka.value,
        model: model.value,
        rule: rulesChecked.value,
        fio: localStorage.getItem('fio'),
        adress: adress.value,
    }
    
    await axios.post('http://localhost:1488/add_request', data, config)
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