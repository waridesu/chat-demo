<script setup>
import {ref} from 'vue';
import Message from '../message/message.vue';
import InputArea from '../input-area/input-area.vue';

const messages = ref([
  {text: 'Hi!', sender: 'Anna', file: null, isImage: false, time: '10:00'},
  {text: 'How are you?', sender: 'Denis', time: '10:01'},
]);

const data = () => {
  return {
    uploadedFile: null,
    isImage: false
  };
}

const handleFileUploaded = (fileData) => {
  console.log(fileData);
}

const addMessage = (message) => {
  messages.value.push({
    text: message.text,
    sender: message.sender,
    time: new Date().toLocaleTimeString()
  });
}
</script>

<template>
  <div class="chat-window">
    <div class="messages">
      <Message
          v-for="(message, index) in messages"
          :key="index"
          :text="message.text"
          :time="message.time"
          :sender="message.sender"/>
    </div>
    <InputArea @send="addMessage" @file-uploaded="handleFileUploaded"/>
  </div>
</template>

<style scoped>
.chat-window {
  margin: 0 auto;
  border: 1px solid #ccc;
  padding: 16px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
}

.messages {
  max-height: 400px;
  overflow-y: auto;
}
</style>
