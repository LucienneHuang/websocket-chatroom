<template>
  <section class="h-dvh w-dvw bg-zinc-900 flex justify-center items-center">
    <div class="text-white grid grid-rows-3 gap-4 rounded-2xl p-6">
      <div class="text-center flex justify-center items-center text-lg">請輸入暱稱</div>
      <input
        v-model="tempUsername"
        class="bg-white rounded-2xl text-2xl text-zinc-900 px-4 py-1"
        type="text"
      />
      <button
        @click="login"
        class="bg-zinc-600 hover:bg-zinc-500 transition-colors duration-200 rounded-2xl cursor-pointer"
      >
        登入
      </button>
    </div>
  </section>
</template>
<script setup>
import { ref } from 'vue'
const emit = defineEmits(['login'])
const tempUsername = ref('')
const login = () => {
  if (tempUsername.value) {
    emit('login', tempUsername.value)
  }
}
</script>
<style scoped></style>
