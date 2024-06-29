<template>
  <div id="app">
    <EnterName v-if="!userName || !userAvatar" @user-entered="setUser" />
    <ChatWindow v-else :userName="userName" :userAvatar="userAvatar" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import EnterName from './components/EnterName.vue';
import ChatWindow from './components/ChatWindow.vue';

export default defineComponent({
  name: 'App',
  components: {
    EnterName,
    ChatWindow,
  },
  setup() {
    const userName = ref(localStorage.getItem('userName') || '');
    const userAvatar = ref(localStorage.getItem('avatar') || '');

    const setUser = (user: { name: string; avatar: string }) => {
      userName.value = user.name;
      userAvatar.value = user.avatar;
    };

    return {
      userName,
      userAvatar,
      setUser,
    };
  },
});
</script>

<style>
#app {
  font-family: Arial, sans-serif;
}
</style>
