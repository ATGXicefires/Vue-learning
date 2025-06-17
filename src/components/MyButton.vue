<script setup>
const props = defineProps({
  // 這裡可以定義組件的 props
  text: String,
})

const emit = defineEmits(['Ciallo'])

function handleclick() {
  // 當按鈕被點擊時，觸發 Ciallo 事件
  emit('Ciallo', props.text)
}
</script>

<template>
  <button class="my-button" @click="handleclick">{{ text }}</button>
</template>

<style scoped>
.my-button {
  background-color: #42b883; /* Vue 的綠色 */
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.my-button:hover {
  background-color: #349668;
}
</style>
