<template>
    <div>
        <h1>Страница заявок</h1>
        <button @click="router.push('/create')">Создать новую заявку</button>
    </div>
    <div v-if="table">
        <h2>{{ username === 'admin' ? 'Все' : 'Мои' }} заявки</h2>
        <table class="table">
            <th>iD заявки</th>
            <th>фИО</th>
            <th>Марка</th>
            <th>Машина</th>
            <th>Дата</th>
            <th>Время</th>
            <th>Адрес</th>
            <th>Статус</th>
            <tr v-for="(item, index) in table" :key="index">
                <td>{{ item.id }}</td>
                <td>{{ item.fio }}</td>
                <td>{{ item.model }}</td>
                <td>{{ item.car }}</td>
                <td>{{ item.data }}</td>
                <td>{{ item.time }}</td>
                <td>{{ item.adress }}</td>
                <td v-if="item.status === null">
                    <p>Статус: Ожидает</p>
                    <button v-if="username === 'admin'" @click="changeStatus('Рассмотрено', item.id)">Рассмотрено</button>
                    <button v-if="username === 'admin'" @click="changeStatus('Отменено', item.id)">Отменено</button>
                </td>
                <td v-if="item.status === 'Рассмотрено'">
                    <p>Статус: Рассмотрено</p>
                    <button v-if="username === 'admin'" @click="changeStatus(null, item.id)">Ожидает</button>
                    <button v-if="username === 'admin'" @click="changeStatus('Отменено', item.id)">Отменено</button>
                </td>
                <td v-if="item.status === 'Отменено'">
                    <p>Статус: Отменено</p>
                    <button v-if="username === 'admin'" @click="changeStatus(null, item.id)">Ожидает</button>
                    <button v-if="username === 'admin'" @click="changeStatus('Рассмотрено', item.id)">Рассмотрено</button>
                </td>
            </tr>
        </table>
    </div>
</template>

<script setup lang="ts">
import router from '@/router';
import axios from 'axios';
import { onMounted, ref } from 'vue';

interface TableData {
    id: number;
    fio: string;
    model: string;
    car: string;
    data: string;
    time: string;
    adress: string;
    status: string;
}

const table = ref<TableData>([])
const username = localStorage.getItem('username')

onMounted(()=>{
    getData()
})

async function getData() {
    const config = {
        headers: {
            Authorization: `Bearer ${localStorage.getItem('token')}`
        }
    }

    const data = {
        fio: localStorage.getItem('fio'),
    }

    const response = await axios.post('http://localhost:1488/requests', data, config)
    table.value = response.data
}

async function changeStatus(id: number, status: string) {
    const config = {
        headers: {
            Authorization: `Bearer ${localStorage.getItem('token')}`
        }
    }

    await axios.post('http://localhost:1488/change_status', {id: id, status: status}, config)
    getData()
}
</script>
<style scoped>
.table{
    border: solid 3px black;
    margin: 0 auto;
    border-collapse: collapse;
    padding: 3px;
}

th,td {
    border: solid 1px black;
    margin: 3px;
    padding: 3px;
}

</style>
