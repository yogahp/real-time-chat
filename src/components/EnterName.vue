<template>
  <div class="enter-name">
    <h2>Enter Your Name</h2>
    <input v-model="name" placeholder="Your name" />
    <h3>Select an Avatar</h3>
    <div class="avatars">
      <div
        v-for="(avatar, index) in avatars"
        :key="index"
        :class="['avatar-option', selectedAvatar === avatar ? 'selected' : '']"
        @click="selectAvatar(avatar)"
      >
        <img :src="avatar" alt="Avatar" />
      </div>
    </div>
    <button @click="joinChat">Join Chat</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'EnterName',
  setup(_, { emit }) {
    const name = ref('');
    const avatars = ref([
      'https://www.gravatar.com/avatar/1?d=identicon',
      'https://www.gravatar.com/avatar/2?d=identicon',
      'https://www.gravatar.com/avatar/3?d=identicon',
      'https://www.gravatar.com/avatar/4?d=identicon',
    ]);
    const selectedAvatar = ref('');

    const selectAvatar = (avatar: string) => {
      selectedAvatar.value = avatar;
    };

    const joinChat = () => {
      if (name.value.trim() !== '' && selectedAvatar.value !== '') {
        localStorage.setItem('userName', name.value);
        localStorage.setItem('avatar', selectedAvatar.value);
        emit('user-entered', { name: name.value, avatar: selectedAvatar.value });
      }
    };

    return {
      name,
      avatars,
      selectedAvatar,
      selectAvatar,
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

.avatars {
  display: flex;
  gap: 10px;
  margin: 10px 0;
}

.avatar-option {
  cursor: pointer;
  border: 2px solid transparent;
  border-radius: 50%;
  padding: 5px;
}

.avatar-option.selected {
  border-color: #3f51b5;
}

.avatar-option img {
  width: 50px;
  height: 50px;
  border-radius: 50%;
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
