<template>
  <div class="container">
    <h1>Week2 Homework</h1>
    <h2>註冊功能</h2>
    <label for="email">電子信箱</label>
    <input
      type="email"
      id="email"
      placeholder="請輸入Email"
      class="me-5"
      v-model="signUpField.email"
    />
    <label for="password">密碼</label>
    <input
      type="text"
      id="password"
      placeholder="請輸入密碼"
      class="me-5"
      v-model="signUpField.password"
    />
    <label for="nickname">暱稱</label>
    <input
      type="text"
      id="nickname"
      placeholder="請輸入暱稱"
      class="me-5"
      v-model="signUpField.nickname"
    />
    <button type="button" @click="signUp">註冊</button>
    <br />
    <p>註冊輸入資訊：{{ signUpField }}</p>
    <p>UID：{{ signUpRes }}</p>
    <br />
    <br />
    <h2>登入功能</h2>
    <label for="email1">電子帳號</label>
    <input
      type="email"
      id="email1"
      placeholder="請輸入Email"
      class="me-5"
      v-model="signInField.email"
    />
    <label for="password1">密碼</label>
    <input
      type="email"
      id="password1"
      placeholder="請輸入password"
      class="me-5"
      v-model="signInField.password"
    />
    <button type="button" @click="signIn">登入</button>
    <br />
    <p>登入輸入資訊：{{ signInField }}</p>
    <p>token：{{ signInRes }}</p>
    <br />
    <h2>驗證</h2>
    {{ user }}
    <div v-if="user && user.uid">
      <p>Uid：{{ user.uid }}</p>
      <p>NickName{{ user.nickname }}</p>
    </div>
    <div v-else>
      <p>你還沒有登入喔</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const api = 'https://todolist-api.hexschool.io/'

//註冊
const signUpField = ref({
  email: '',
  password: '',
  nickname: '',
})

const signUpRes = ref('')
const signUp = async () => {
  //console.log(`${api}users/sign_up`)
  try {
    const res = await axios.post(`${api}users/sign_up`, signUpField.value)
    console.log(res)
    signUpRes.value = res.data.uid
  } catch (error) {
    console.log(error)
    console.log('有錯誤喔')
  }
}

//登入
const signInField = ref({
  email: '',
  password: '',
})

const signInRes = ref('')
const signIn = async () => {
  try {
    // console.log(`${api}users/sign_in`)
    const res = await axios.post(`${api}users/sign_in`, signInField.value)
    signInRes.value = res.data.token
    document.cookie = `custom1TodoToken=${signInRes.value};path=/`
  } catch (error) {
    console.log(error)
    console.log('有錯誤喔')
  }
}

//驗證
const user = ref({
  uid: '',
  nickname: '',
})

onMounted(async () => {
  const token = document.cookie.replace(/(?:^|.*;\s*)custom1TodoToken\s*=\s*([^;]*).*$/i, '$1')
  const res = await axios.get(`${api}users/checkout`, {
    headers: {
      Authorization: token,
    },
  })
  console.log(res)
  user.value = res.data
})
</script>

<style>
.container {
  max-width: 1296px;
  margin: 0 auto;
}

.me-5 {
  margin-right: 20px;
}
</style>
