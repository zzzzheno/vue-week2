<script setup>
import { ref } from 'vue'
import axios from 'axios'

const tem = ref('signIn')
</script>

<template>
  <div class="container d-flex justify-content-center py-5">
    <div class="row p-5 todoList" style="width: 500px">
      <div class="col-12">
        <h1 class="text-center fw-bold py-4">Todo List</h1>
      </div>
      <!-- 登入 開始 -->
      <div class="col-12 py-5" v-if="tem === 'signIn'">
        <h3 class="text-center fw-bold py-3">登入</h3>
        <form>
          <div class="d-flex flex-column mb-4">
            <label class="mb-1 fw-bold" for="signInAccount">帳號</label>
            <input type="email" id="signInAccount" placeholder="請輸入帳號" />
          </div>
          <div class="d-flex flex-column mb-5">
            <label class="mb-1 fw-bold" for="signInPassword">密碼</label>
            <input type="password" id="singInPassword" placeholder="請輸入密碼" />
          </div>
          <div class="d-flex">
            <button class="me-3 thirdButton" type="button">註冊</button>
            <button class="flex-grow-1 mainButton" type="button">登入</button>
          </div>
        </form>
      </div>
      <!-- 登入 結束 -->

      <!-- 註冊 開始 -->
      <div class="col-12 py-5" v-else-if="tem === 'register'">
        <h3 class="text-center fw-bold py-3">註冊</h3>
        <form>
          <div class="d-flex flex-column mb-4">
            <label class="mb-1 fw-bold" for="registerAccount">帳號</label>
            <input type="email" id="registerAccount" placeholder="請輸入帳號" />
          </div>
          <div class="d-flex flex-column mb-4">
            <label class="mb-1 fw-bold" for="registerPassword">密碼</label>
            <input type="password" id="registerPassword" placeholder="請輸入密碼" />
          </div>
          <div class="d-flex flex-column mb-5">
            <label class="mb-1 fw-bold" for="registerNickName">暱稱</label>
            <input type="text" id="registerNickName" placeholder="請輸入暱稱" />
          </div>
          <div class="d-flex">
            <button class="me-3 thirdButton" type="button">返回登入</button>
            <button class="flex-grow-1 mainButton" type="button">確定註冊</button>
          </div>
        </form>
      </div>
      <!-- 註冊 結束 -->
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
