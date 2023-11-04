<script setup lang="ts">
import { ref } from 'vue';
import EmojiPicker, { EmojiExt } from 'vue3-emoji-picker'
import 'vue3-emoji-picker/css'

const emit = defineEmits(['send', 'file-uploaded']);
const currentMessage = ref('');
const isPickerOpen = ref(false);
const uploadedFile = ref<File | null>(null);
const isImage = ref(false);
const fileInput = ref<HTMLInputElement | null>(null);
const triggerFileInput = () => {
  if(fileInput.value) {
    fileInput.value.click();
  }
}
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
const handleFileUpload = (event: Event) => {
  const input = event.target as HTMLInputElement;
  if (!input.files) return;

  const file = input.files[0];
  uploadedFile.value = file;

  const acceptedImageTypes = ['image/gif', 'image/jpeg', 'image/png'];
  isImage.value = acceptedImageTypes.includes(file.type);

  emit('file-uploaded', { file, isImage: isImage.value });
}

const onSelectEmoji = (emoji: EmojiExt) => {
  currentMessage.value += emoji.i;
  isPickerOpen.value = false;
}
</script>

<template>
  <div class="input-area">
    <label>
      <input v-model="currentMessage" @keyup.enter="sendMessage" placeholder='Enter message...'/>
      <button @click="togglePicker">😀</button>
      <EmojiPicker class="emoji-picker-modal" v-if="isPickerOpen" :native="true" @select="onSelectEmoji"/>
      <img src="../../assets/attachment.svg" alt="Upload" @click="triggerFileInput" class="upload-button" />
    </label>
    <button class="button" @click="sendMessage">Send</button>
    <input type="file" ref="fileInput" @change="handleFileUpload" class="hidden"/>

  </div>
</template>

<style scoped>
.input-area {
  display: flex;
  align-items: center;
  margin-top: 16px;
  gap: 10px;
}

button {
  margin: 0;
  padding: 0;
  border: none;
  background: none;
  cursor: pointer;
}

label {
  position: relative;
  display: flex;
  align-items: center;
  flex: 1;
  margin-right: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  resize: vertical;
  background-color: #3b3b3b;
}

input {
  padding: 8px;
  border: none;
}

.button {
  padding: 8px 16px;
  border: none;
  background-color: #007bff;
  color: #fff;
  cursor: pointer;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.emoji-picker-modal {
  position: absolute;
  bottom: 40px;
  left: 10px;
}

button:hover {
  background-color: #0056b3;
}

.hidden {
  display: none;
}
.upload-button {
  cursor: pointer;
  height: 19px;
  aspect-ratio: 1;
  margin-inline: 8px;
}
</style>
