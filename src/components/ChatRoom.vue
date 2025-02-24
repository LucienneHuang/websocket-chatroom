<template>
  <!-- 聊天室 -->
  <section class="h-dvh w-dvw bg-zinc-800 flex flex-col">
    <div class="bg-zinc-900 text-white backdrop-blur-lg w-full p-4 text-lg text-center relative">
      聊天室
      <button class="absolute right-4 cursor-pointer" @click="logout">
        <i class="fa-solid fa-right-from-bracket"></i>
      </button>
    </div>
    <div class="bg-zinc-50 flex-1 overflow-y-scroll py-2">
      <div
        class="message"
        v-for="(message, index) in messages"
        :class="[message.user === username ? 'my-message' : 'others-message']"
        :key="index"
      >
        <template v-if="message.user">
          <div class="user-message p-2">
            <div class="user-message__user">{{ message.user }}</div>
            <div class="user-message__text">
              {{ message.text }}
            </div>
          </div>
        </template>
        <div class="system-message text-zinc-800 mx-4 px-2 py-1 font-black text-center" v-else>
          {{ message.text }}
        </div>
      </div>
    </div>
    <div class="h-30 p-4 flex bg-white">
      <textarea
        v-model="tempMessage"
        name=""
        id=""
        class="flex-1 bg-white rounded-3xl drop-shadow-lg p-3"
      ></textarea>
      <button @click="sendMessage" class="ml-4 mr-1">
        <i class="fa-solid fa-paper-plane"></i>
      </button>
    </div>
  </section>
</template>
<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
const props = defineProps(['username'])
const emit = defineEmits(['logout'])
const tempMessage = ref('')
const messages = ref([])
const socket = ref(null)
const sendMessage = () => {
  const messageInfo = {
    type: 'message',
    user: props.username,
    text: tempMessage.value,
  }
  socket.value.send(JSON.stringify(messageInfo))
  tempMessage.value = ''
}
const connectWebsocket = () => {
  socket.value = new WebSocket(import.meta.env.VITE_API)
  socket.value.onopen = () => {
    // console.log('websocket 連線成功', socket.value.readyState);
    const userInfo = {
      type: 'join',
      user: props.username,
    }
    socket.value.send(JSON.stringify(userInfo))
  }
  socket.value.onmessage = (event) => {
    // console.log('收到 message', event);
    const message = JSON.parse(event.data)
    messages.value.push(message)
  }
  socket.value.onclose = () => {
    // console.log('連線關閉中1', socket.value.readyState);
  }
}
const logout = () => {
  console.log('hi')
  socket.value.close()
  console.log('close')
  emit('logout')
}
onMounted(() => {
  connectWebsocket()
})
onUnmounted(() => {
  if (socket.value) socket.value.close()
})
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
