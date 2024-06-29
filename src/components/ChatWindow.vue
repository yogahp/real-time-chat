<template>
  <div class="chat-window">
    <div class="messages">
      <div
        v-for="(message, index) in messages"
        :key="index"
        :class="['message', message.sender === user ? 'sent' : 'received']"
      >
        {{ message.text }}
      </div>
    </div>
    <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="Type a message" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, inject, onMounted } from 'vue';

interface Message {
  text: string;
  sender: string;
}

export default defineComponent({
  name: 'ChatWindow',
  setup() {
    const newMessage = ref('');
    const messages = ref<Message[]>([]);
    const socket = inject('socket') as any;
    const user = ref(localStorage.getItem('user') || 'user1');

    onMounted(() => {
      if (!localStorage.getItem('user')) {
        localStorage.setItem('user', user.value);
      }

      socket.on('chat message', (msg: string, sender: string) => {
        if (sender !== user.value) {
          messages.value.push({ text: msg, sender });
        }
      });
    });

    const sendMessage = () => {
      if (newMessage.value.trim() !== '') {
        const messageText = newMessage.value;
        newMessage.value = '';
        messages.value.push({ text: messageText, sender: user.value });
        socket.emit('chat message', messageText, user.value);
      }
    };

    return {
      newMessage,
      messages,
      sendMessage,
      user,
    };
  },
});
</script>

<style scoped>
.chat-window {
  display: flex;
  flex-direction: column;
  height: 400px;
  width: 300px;
  border: 1px solid #ccc;
  padding: 10px;
  background-color: #fff;
}
.messages {
  flex: 1;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}
.message {
  padding: 5px;
  margin-bottom: 10px;
  border-radius: 10px;
  max-width: 80%;
  word-wrap: break-word;
}
.sent {
  background-color: #e1ffc7;
  align-self: flex-end;
  text-align: right;
}
.received {
  background-color: #f1f0f0;
  align-self: flex-start;
  text-align: left;
}
input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-top: 10px;
}
</style>
