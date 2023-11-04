<script setup lang="ts">
import { computed } from "vue";

const props = defineProps({
  text: {
    type: String,
    required: true
  },
  sender: {
    type: String,
    required: true
  },
  time: {
    type: String,
    required: true
  }
});

const isSender = computed(() => props.sender === 'You');
</script>

<template>
  <div class="message" :class="{ 'message-sender': isSender }">
    <p><strong>{{ sender }}</strong> | {{ time }}: {{ text }}</p>
  </div>
</template>

<style scoped>
.message {
  padding: 8px 16px;
  margin-bottom: 8px;
  text-align: left;
  background-color: #9a9a9a;
  border-radius: 4px;
}
.message-sender {
  background-color: #007bff;
  color: #fff;
  text-align: right;
}
</style>
