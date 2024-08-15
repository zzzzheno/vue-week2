<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'

const site = 'https://todolist-api.hexschool.io'

// 登入的模板
const tem = ref('signInTem')
// 登入的模板

const email = ref('')
const password = ref('')
const nickName = ref('')
const message = ref('')
const token = ref('')

// 登入
const signIn = async () => {
  try {
    const response = await axios.post(`${site}/users/sign_in`, {
      email: email.value,
      password: password.value
    })
    token.value = response.data.token
    localStorage.setItem('token', token.value)
    tem.value = 'todoTem'
  } catch (error) {
    alert((message.value = error.response.data.message))
  }
}
// 登入 結束

// 驗證 開始
const tokenCheck = ref('')

const checkToken = async () => {
  const tomorrow = new Date()
  tomorrow.setDate(tomorrow.getDate() + 1)
  document.cookie = `todoList=${tokenCheck.value}; expires=${tomorrow.toUTCString()}`
  try {
    const response = await axios.get(`${site}/users/checkout`, {
      headers: {
        Authorization: tokenCheck.value
      }
    })
    token.value = tokenCheck.value
    tem.value = 'todoTem'
  } catch (error) {
    message.value = '驗證失敗'
  }
}
// 驗證 結束

onMounted(() => {
  checkToken()
})

// 註冊 開始
// todo:補上欄位驗證
const register = async () => {
  try {
    const response = await axios.post(`${site}/users/sign_up`, {
      email: email.value,
      password: password.value,
      nickname: nickName.value
    })
    alert((message.value = `${nickName.value}註冊成功！請返回登入頁登入`))
  } catch (error) {
    alert((message.value = 'oops!' + error.response.data.message))
  }
  ;(email.value = ''), (password.value = ''), (nickName.value = '')
}
// 註冊 結束

// 登出 開始
const signOut = async () => {
  try {
    const response = await axios.post(
      `${site}/users/sign_out`,
      {},
      {
        headers: {
          Authorization: token.value
        }
      }
    )
    localStorage.removeItem('token')
    token.value = ''
    tem.value = 'signInTem'
    alert((message.value = '登出成功'))
  } catch (error) {
    alert((message.value = '登出失敗'))
  }
}
// 登出 結束
</script>

<template>
  <div class="container d-flex justify-content-center py-5">
    <div class="row p-5 todoList" style="width: 500px">
      <div class="col-12">
        <h1 class="text-center fw-bold py-2">Todo List</h1>
      </div>
      <!-- 登入 開始 -->
      <div class="col-12 py-2" v-if="tem === 'signInTem'">
        <h3 class="text-center fw-bold py-3">登入</h3>
        <form>
          <div class="d-flex flex-column mb-4">
            <label class="mb-1 fw-bold" for="signInAccount">帳號</label>
            <input type="email" id="signInAccount" placeholder="請輸入帳號" v-model="email" />
          </div>
          <div class="d-flex flex-column mb-5">
            <label class="mb-1 fw-bold" for="signInPassword">密碼</label>
            <input
              type="password"
              id="singInPassword"
              placeholder="請輸入密碼"
              v-model="password"
            />
          </div>
          <div class="d-flex">
            <button
              class="me-3 thirdButton"
              type="button"
              :class="{ active: tem === 'registerTem' }"
              @click="tem = 'registerTem'"
            >
              註冊
            </button>
            <button class="flex-grow-1 mainButton" type="button" @click="signIn">登入</button>
          </div>
        </form>
      </div>
      <!-- 登入 結束 -->

      <!-- 註冊 開始 -->
      <div class="col-12 py-2" v-else-if="tem === 'registerTem'">
        <h3 class="text-center fw-bold py-3">註冊</h3>
        <form>
          <div class="d-flex flex-column mb-4">
            <label class="mb-1 fw-bold" for="registerAccount">帳號</label>
            <input type="email" id="registerAccount" placeholder="請輸入帳號" v-model="email" />
          </div>
          <div class="d-flex flex-column mb-4">
            <label class="mb-1 fw-bold" for="registerPassword">密碼</label>
            <input
              type="password"
              id="registerPassword"
              placeholder="請輸入密碼"
              v-model="password"
            />
          </div>
          <div class="d-flex flex-column mb-5">
            <label class="mb-1 fw-bold" for="registerNickName">暱稱</label>
            <input type="text" id="registerNickName" placeholder="請輸入暱稱" v-model="nickName" />
          </div>
          <div class="d-flex">
            <button
              class="me-3 thirdButton"
              type="button"
              :class="{ active: tem === 'signInTem' }"
              @click="tem = 'signInTem'"
            >
              返回登入
            </button>
            <button class="flex-grow-1 mainButton" type="button" @click="register">確定註冊</button>
          </div>
        </form>
      </div>
      <!-- 註冊 結束 -->

      <!-- Todo頁 開始 -->
      <div class="col-12 py-2" v-else-if="tem === 'todoTem'">
        <h3 class="text-center fw-bold py-3">Todo 頁</h3>
        <button type="button" class="thirdButton" @click="signOut">登出</button>
      </div>
      <!-- Todo頁 結束 -->
    </div>
  </div>
</template>

<style scoped>
body {
  letter-spacing: 0.1rem;
}

input {
  font-size: 1rem;
  padding: 0.5rem 1rem;
  border: solid 1px #000;
  box-shadow: 1px 1px 0 #000;
}

::placeholder {
  color: #7c7c7c;
}

.todoList {
  background: #fff;
  border: solid 1px #000;
  box-shadow: 1px 1px 0 #000;
}

.mainButton {
  background-color: #000;
  font-size: 1.125rem;
  font-weight: 700;
  color: #fff;
  padding: 0.5rem 1rem;
  border: solid 1px #000;
  box-shadow: 1px 1px 0 #000;
  transition-duration: 1s;
}

.mainButton:hover {
  background-color: #fff;
  color: #000;
}

.thirdButton {
  background-color: #fff;
  font-size: 1.125rem;
  font-weight: 700;
  color: #000;
  padding: 0.5rem 1rem;
  border: solid 1px #000;
  box-shadow: 1px 1px 0 #000;
  transition-duration: 1s;
}

.thirdButton:hover {
  background-color: #000;
  color: #fff;
}
</style>
