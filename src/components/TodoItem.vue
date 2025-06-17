<script setup>
const props = defineProps({
  todo: {
    type: Object,
    required: true,
  },
})
const emit = defineEmits(['delete-todo', 'toggle-complete'])
function handleDelete() {
  emit('delete-todo', props.todo.id)
}
function handleToggle() {
  emit('toggle-complete', props.todo.id)
}
</script>

<template>
  <li class="todo-item" :class="{ completed: todo.completed }">
    <div class="context">
      <input
        type="checkbox"
        :checked="todo.completed"
        @change="handleToggle"
        :id="'todo-' + todo.id"
      />
      <label :for="'todo-' + todo.id">{{ todo.text }}</label>
      <button @click="handleDelete" class="delete-btn">刪除</button>
    </div>
  </li>
</template>

<style scoped>
.context {
  display: flex;
  align-items: center;
  width: 100%;
}
.context label {
  cursor: pointer;
  margin-left: 10px;
}
.todo-item {
  padding: 12px 15px;
  background-color: #f2f2f2;
  border-radius: 8px;
  margin-bottom: 10px;
  font-size: 1.1rem;
  color: #333; /* 深灰色文字 */
  display: flex;
  align-items: center;
  transition:
    opacity 0.3s,
    background-color 0.3s;
}
.todo-item.completed {
  opacity: 0.6;
  background-color: #e0e0e0;
}
.todo-item.completed span {
  text-decoration: line-through;
  color: #999; /* 淺灰色文字 */
}
input[type='checkbox'] {
  margin-right: 10px;
  cursor: pointer;
}
.todo-item {
  position: relative; /* 為了讓刪除按鈕可以定位 */
}
.delete-btn {
  background: none;
  border: none;
  color: #cc0000;
  font-size: 20px;
  cursor: pointer;
  opacity: 0;
  transition: opacity 0.3s;
}
.todo-item:hover .delete-btn {
  opacity: 1;
}
.todo-item:hover .delete-btn {
  opacity: 1; /* 滑鼠移過時才顯示刪除按鈕 */
}
</style>
