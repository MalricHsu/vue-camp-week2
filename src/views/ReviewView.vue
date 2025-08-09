<template>
  <h1>W1複習+computed+onMounted</h1>
  <div>
    <input type="text" placeholder="輸入產品名稱" v-model="newName" />
    {{ newName }}
    <input type="number" placeholder="輸入產品價格" v-model="newNumber" />
    {{ newNumber }}
    <button type="button" v-on:click="addProduct">新增到資料集裡面</button>
  </div>
  <br />
  <hr />
  <table>
    <thead>
      <tr>
        <th>標題</th>
        <th>價格</th>
        <th>調整價格</th>
        <th>刪除</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in goodsData" v-bind:key="item.id">
        <td>{{ item.name }}</td>
        <td>{{ item.price }}</td>
        <td><input type="number" v-model="item.price" /></td>
        <td><button type="button" v-on:click="delItem(item.id)">刪除品項</button></td>
      </tr>
    </tbody>
  </table>
  <br />
  <hr />
  <h2>全部商品總額：{{ sum }}元</h2>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const newName = ref('')
const newNumber = ref('')

const goodsData = ref([])

const data = [
  { id: 1, name: '珍珠奶茶', price: 50 },
  { id: 2, name: '冬瓜檸檬', price: 45 },
  { id: 3, name: '翡翠檸檬', price: 55 },
  { id: 4, name: '四季春茶', price: 45 },
  { id: 5, name: '阿薩姆奶茶', price: 50 },
  { id: 6, name: '檸檬冰茶', price: 45 },
  { id: 7, name: '芒果綠茶', price: 55 },
  { id: 8, name: '抹茶拿鐵', price: 60 },
]

const addProduct = () => {
  goodsData.value.push({
    id: Date.now(), // 用時間戳作 id
    name: newName.value,
    price: Number(newNumber.value), // 確保是數字
  })
  newName.value = ''
  newNumber.value = ''
}

const delItem = (id) => {
  const Index = goodsData.value.findIndex((item) => item.id === id)
  if (Index !== -1) {
    goodsData.value.splice(Index, 1)
  }
}

const sum = computed(() => {
  let totalPrice = 0
  goodsData.value.forEach((item) => {
    totalPrice += Number(item.price)
  })
  return totalPrice
})

onMounted(() => {
  setTimeout(() => {
    goodsData.value = data
  }, 5000)
})
</script>

<style></style>
