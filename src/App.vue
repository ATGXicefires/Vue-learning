<script setup>
import { ref } from 'vue'
import TodoItem from './components/TodoItem.vue'

let nextTodoId = 5 // 下一個待辦事項的 ID
//學列表
const todos = ref([
  { id: 1, text: '學習 Vue 的基礎知識', completed: false },
  { id: 2, text: '學習組件化開發', completed: false },
  { id: 3, text: '學習列表渲染 v-for', completed: false },
  { id: 4, text: '接下來要學習條件渲染 v-if', completed: false },
])

const newTodoText = ref('')

function addTodo() {
  if (newTodoText.value.trim() === '') return
  todos.value.push({
    id: nextTodoId++,
    text: newTodoText.value.trim(),
    completed: false,
  })
  newTodoText.value = ''
}

function handleToggleComplete(id) {
  const todo = todos.value.find((todo) => todo.id === id)
  if (todo) {
    todo.completed = !todo.completed
  }
}

function deleteTodo(id) {
  todos.value = todos.value.filter((todo) => todo.id !== id)
}
</script>

<template>
  <div class="todo-list">
    <h1>我的待辦事項</h1>
    <div class="add-todo-section">
      <form @submit.prevent="addTodo">
        <input v-model.lazy="newTodoText" type="text" placeholder="新增待辦事項" required />
        <button type="submit">新增</button>
      </form>
      <!-- v-if 現在會被正確地推到表單的下一行 -->
      <p v-if="todos.length === 0">目前沒有待辦事項</p>
    </div>
    <ul>
      <TodoItem
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @toggle-complete="handleToggleComplete"
        @delete-todo="deleteTodo"
      />
    </ul>
  </div>
</template>

<style scoped>
h1 {
  text-align: center;
  color: #42b883;
  margin-bottom: 1.5rem;
}
.add-todo-section {
  margin-bottom: 1.5rem;
  text-align: center;
}

.add-todo-section p {
  text-align: center;
  margin-top: 1rem;
  color: #888;
}

.add-form input {
  flex-grow: 1; /* 保持這個，讓它自動填滿空間 */
  border: 1px solid #ccc;
  border-radius: 8px 0 0 8px;

  /* ⭐ 新增或修改這些屬性 */
  padding: 12px 15px; /* 上下 12px，左右 15px 的內邊距 */
  font-size: 1rem; /* 確保字體大小適中 */
  outline: none; /* 點擊時移除瀏覽器預設的藍色外框，可選 */
}

.add-form input:focus {
  border-color: #42b883; /* 點擊輸入框時，讓邊框變色，提升體驗 */
}

.add-form button {
  padding: 12px 20px; /* 讓按鈕和輸入框高度看起來一致 */
  background-color: #42b883;
  color: white;
  border: none;
  border-radius: 0 8px 8px 0;
  cursor: pointer;
  font-size: 1rem;
}
.empty-state {
  text-align: center;
  color: #777;
  margin-top: 2rem;
}
</style>
