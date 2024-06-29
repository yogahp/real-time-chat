<template>
  <div class="enter-name">
    <h2>Enter Your Name</h2>
    <input v-model="name" placeholder="Your name" />
    <button @click="joinChat">Join Chat</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'EnterName',
  setup(_, { emit }) {
    const name = ref('');

    const joinChat = () => {
      if (name.value.trim() !== '') {
        localStorage.setItem('userName', name.value);
        emit('user-entered', name.value);
      }
    };

    return {
      name,
      joinChat,
    };
  },
});
</script>

<style scoped>
.enter-name {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  font-family: Arial, sans-serif;
}

input {
  padding: 10px;
  font-size: 16px;
  margin: 10px 0;
  border-radius: 5px;
  border: 1px solid #ccc;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  background-color: #3f51b5;
  color: white;
  border-radius: 5px;
  cursor: pointer;
}
</style>
