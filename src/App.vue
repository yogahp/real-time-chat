<template>
  <div id="app">
    <EnterName v-if="!userName" @user-entered="setUserName" />
    <ChatWindow v-else :userName="userName" />
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

    const setUserName = (name: string) => {
      userName.value = name;
    };

    return {
      userName,
      setUserName,
    };
  },
});
</script>

<style>
#app {
  font-family: Arial, sans-serif;
}
</style>
