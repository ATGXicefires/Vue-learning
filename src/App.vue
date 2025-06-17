<script setup>
import { ref, computed } from 'vue'
import TodoItem from './components/TodoItem.vue'

let nextTodoId = 11 // 下一個待辦事項的 ID
//學列表
const todos = ref([
  { id: 1, text: '學習 Vue 的基礎知識', completed: false },
  { id: 2, text: '響應式資料 (ref)', completed: false },
  { id: 3, text: '雙向綁定 (v-model 和 .lazy 修飾符)', completed: false },
  { id: 4, text: '組件化開發 (.vue 單一檔案組件)', completed: false },
  { id: 5, text: '父子組件通訊 (props 向下傳遞, emits 向上通知)', completed: false },
  { id: 6, text: '列表渲染 (v-for 和 :key)', completed: false },
  { id: 7, text: '條件渲染 (v-if, v-else)', completed: false },
  { id: 8, text: '動態樣式綁定 (:class)', completed: false },
  { id: 9, text: '事件處理 (@click, @submit.prevent)', completed: false },
  { id: 10, text: '計算屬性 (computed)', completed: false },
])

const newTodoText = ref('')

const visibility = ref('all') // 可選值：'all', 'active', 'completed'

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

const filteredTodos = computed(() => {
  switch (visibility.value) {
    case 'active':
      return todos.value.filter((todo) => !todo.completed)
    case 'completed':
      return todos.value.filter((todo) => todo.completed)
    default:
      return todos.value // 'all' 或其他情況，返回所有待辦事項
  }
})
</script>

<template>
  <div class="todo-list">
    <h1>我的待辦事項</h1>
    <div class="add-todo-section">
      <form class="add-form" @submit.prevent="addTodo">
        <input v-model.lazy="newTodoText" type="text" placeholder="新增待辦事項" required />
        <button type="submit">新增</button>
      </form>
    </div>
    <div class="filter-controls">
      <button @click="visibility = 'all'" :class="{ active: visibility === 'all' }">全部</button>
      <button @click="visibility = 'active'" :class="{ active: visibility === 'active' }">
        未完成
      </button>
      <button @click="visibility = 'completed'" :class="{ active: visibility === 'completed' }">
        已完成
      </button>
    </div>
    <ul v-if="filteredTodos.length > 0" class="todo-list">
      <TodoItem
        v-for="todo in filteredTodos"
        :key="todo.id"
        :todo="todo"
        @toggle-complete="handleToggleComplete"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-else class="todos.length === 0">目前沒有待辦事項</p>
  </div>
</template>

<style scoped>
h1 {
  text-align: center;
  color: #42b883;
  margin-bottom: 1.5rem;
}
p {
  text-align: center;
  color: #555;
  margin-bottom: 1.5rem;
}
.add-form {
  display: flex;
  justify-content: center;
  align-items: center;
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
.filter-controls {
  display: flex;
  justify-content: center;
  gap: 10px; /* 按鈕之間的間距 */
  margin-bottom: 1.5rem;
}

.filter-controls button {
  padding: 8px 16px;
  border: 1px solid #ddd;
  background-color: white;
  color: #555;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s;
}

.filter-controls button:hover {
  background-color: #f2f2f2;
}

/* 當按鈕處於 active 狀態時的樣式 */
.filter-controls button.active {
  background-color: #42b883;
  color: white;
  border-color: #42b883;
}
</style>
