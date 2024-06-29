<template>
  <div class="home">
    <ChatWindow />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import ChatWindow from '@/components/ChatWindow.vue';

export default defineComponent({
  name: 'HomeView',
  components: {
    ChatWindow,
  },
});
</script>

<style scoped>
.home {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f0f0;
}
</style>
