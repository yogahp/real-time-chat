<template>
  <div class="chat-container">
    <div class="chat-header">
      <h2># general</h2>
    </div>
    <div class="chat-window">
      <div class="messages">
        <div
          v-for="(message, index) in messages"
          :key="index"
          :class="['message', message.sender === userName ? 'sent' : 'received']"
        >
          <div class="avatar" v-if="message.sender !== userName">
            <img src="https://via.placeholder.com/40" alt="Avatar" />
          </div>
          <div class="message-content">
            <div class="message-text">
              <strong>{{ message.sender }}</strong><br>
              {{ message.text }}
            </div>
          </div>
          <div class="avatar" v-if="message.sender === userName">
            <img src="https://via.placeholder.com/40" alt="Avatar" />
          </div>
        </div>
      </div>
      <div class="input-container">
        <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="Type a message" />
        <button @click="sendMessage"><span role="img" aria-label="send">➤</span></button>
      </div>
    </div>
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
  props: {
    userName: {
      type: String,
      required: true,
    },
  },
  setup(props) {
    const newMessage = ref('');
    const messages = ref<Message[]>([]);
    const socket = inject('socket') as any;

    onMounted(() => {
      socket.on('chat message', (msg: string, sender: string) => {
        if (sender !== props.userName) {
          messages.value.push({ text: msg, sender });
        }
      });
    });

    const sendMessage = () => {
      if (newMessage.value.trim() !== '') {
        const messageText = newMessage.value;
        newMessage.value = '';
        messages.value.push({ text: messageText, sender: props.userName });
        socket.emit('chat message', messageText, props.userName);
      }
    };

    return {
      newMessage,
      messages,
      sendMessage,
    };
  },
});
</script>

<style scoped>
.chat-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 100%;
  max-width: 600px;
  border: 1px solid #ccc;
  background-color: #f7f7f7;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  margin: auto;
  font-family: Arial, sans-serif;
}

.chat-header {
  padding: 10px;
  background-color: #3f51b5;
  color: white;
  text-align: center;
}

.chat-window {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
}

.messages {
  flex: 1;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  padding: 10px;
}

.message {
  display: flex;
  align-items: flex-start;
  margin-bottom: 10px;
}

.sent {
  flex-direction: row-reverse;
}

.message-content {
  max-width: 80%;
  margin: 0 10px;
}

.message-text {
  padding: 10px 15px;
  border-radius: 20px;
  font-size: 14px;
  line-height: 1.4;
  background-color: #e1ffc7;
  align-self: flex-end;
  text-align: right;
}

.received .message-text {
  background-color: #f1f0f0;
  align-self: flex-start;
  text-align: left;
}

.avatar img {
  width: 40px;
  height: 40px;
  border-radius: 50%;
}

.input-container {
  display: flex;
  padding: 10px;
  background-color: #fff;
  border-top: 1px solid #ddd;
}

input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 20px;
  outline: none;
  font-size: 14px;
}

button {
  background: none;
  border: none;
  font-size: 24px;
  margin-left: 10px;
  cursor: pointer;
}
</style>
