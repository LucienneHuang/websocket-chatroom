<template>
  <ChatRoom v-if="username" :username="username" @logout="handleLogout" />
  <LoginForm v-else @login="handleLogin" />
</template>
<script setup>
import { ref } from 'vue'
import ChatRoom from '@/components/ChatRoom.vue'
import LoginForm from '@/components/LoginForm.vue'
const username = ref('')

const handleLogin = (name) => {
  username.value = name
}

const handleLogout = () => {
  console.log('clear?')
  username.value = ''
}
</script>
<style scoped>
.my-message .user-message {
  margin: 0rem 0.5rem 0rem 3rem;
}
.my-message .user-message__user {
  text-align: right;
}
.my-message .user-message__text {
  background: lightblue;
  padding: 0.6rem;
  border-top-left-radius: 1.5rem;
  border-bottom-left-radius: 1.5rem;
  border-bottom-right-radius: 1.5rem;
}

.others-message .user-message {
  margin: 0rem 3rem 0rem 0.5rem;
}
.others-message .user-message__text {
  background: rgb(238, 238, 238);
  padding: 0.6rem;
  border-bottom-left-radius: 1.5rem;
  border-bottom-right-radius: 1.5rem;
  border-top-right-radius: 1.5rem;
}
</style>
