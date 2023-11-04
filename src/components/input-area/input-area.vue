<script setup lang="ts">
import { ref } from 'vue';
import EmojiPicker, { EmojiExt } from 'vue3-emoji-picker'
import 'vue3-emoji-picker/css'

const emit = defineEmits(['send']);
const currentMessage = ref('');
const isPickerOpen = ref(false);

const togglePicker = () => {
  isPickerOpen.value = !isPickerOpen.value;
}

const sendMessage = () => {
  if (currentMessage.value.trim() !== "") {
    emit('send', {text: currentMessage.value, sender: 'You'});
    currentMessage.value = '';
    isPickerOpen.value = false;
  }
}
const onSelectEmoji = (emoji: EmojiExt) => {
  currentMessage.value += emoji.i;
  isPickerOpen.value = false;
}
</script>

<template>
  <div class="input-area">
    <input v-model="currentMessage" @keyup.enter="sendMessage" placeholder='Enter message...'/>
    <button @click="sendMessage">Send</button>
    <div class="emoji-picker">
      <button @click="togglePicker">Emoji</button>
      <EmojiPicker class="emoji-picker-modal" v-if="isPickerOpen" :native="true" @select="onSelectEmoji"/>
    </div>
  </div>
</template>

<style scoped>
.input-area {
  display: flex;
  align-items: center;
  margin-top: 16px;
  gap: 10px;
}

input {
  flex: 1;
  padding: 8px;
  margin-right: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  resize: vertical;
}

button {
  padding: 8px 16px;
  border: none;
  background-color: #007bff;
  color: #fff;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.emoji-picker {
  position: relative;
}

.emoji-picker-modal {
  position: absolute;
  bottom: 40px;
}

button:hover {
  background-color: #0056b3;
}
</style>
