<template>
  <div class="todo-app">
    <header class="header">
      <h1 class="title">TodoList</h1>
      <p class="subtitle">Stay organized, stay productive</p>
    </header>

    <div class="todo-container">
      <TodoInput @add-todo="addTodo" />
      
      <div class="stats-bar">
        <span class="task-count">
          {{ activeCount }} {{ activeCount === 1 ? 'task' : 'tasks' }} remaining
        </span>
        <TodoFilters 
          :current-filter="currentFilter" 
          @filter-change="setFilter" 
        />
      </div>

      <TodoList 
        :todos="filteredTodos"
        @toggle-todo="toggleTodo"
        @delete-todo="deleteTodo"
        @edit-todo="editTodo"
      />

      <div class="footer-actions" v-if="todos.length > 0">
        <button 
          class="clear-completed"
          @click="clearCompleted"
          v-if="completedCount > 0"
        >
          Clear completed ({{ completedCount }})
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, watch } from 'vue'
import TodoInput from './TodoInput.vue'
import TodoList from './TodoList.vue'
import TodoFilters from './TodoFilters.vue'
import type { Todo, FilterType } from '../types/todo'

const todos = ref<Todo[]>([])
const currentFilter = ref<FilterType>('all')

const activeCount = computed(() => todos.value.filter(todo => !todo.completed).length)
const completedCount = computed(() => todos.value.filter(todo => todo.completed).length)

const filteredTodos = computed(() => {
  switch (currentFilter.value) {
    case 'active':
      return todos.value.filter(todo => !todo.completed)
    case 'completed':
      return todos.value.filter(todo => todo.completed)
    default:
      return todos.value
  }
})

const addTodo = (text: string) => {
  const newTodo: Todo = {
    id: Date.now(),
    text: text.trim(),
    completed: false,
    createdAt: new Date()
  }
  todos.value.unshift(newTodo)
}

const toggleTodo = (id: number) => {
  const todo = todos.value.find(t => t.id === id)
  if (todo) {
    todo.completed = !todo.completed
  }
}

const deleteTodo = (id: number) => {
  const index = todos.value.findIndex(t => t.id === id)
  if (index > -1) {
    todos.value.splice(index, 1)
  }
}

const editTodo = (id: number, newText: string) => {
  const todo = todos.value.find(t => t.id === id)
  if (todo) {
    todo.text = newText.trim()
  }
}

const setFilter = (filter: FilterType) => {
  currentFilter.value = filter
}

const clearCompleted = () => {
  todos.value = todos.value.filter(todo => !todo.completed)
}

const saveTodos = () => {
  localStorage.setItem('vue-todos', JSON.stringify(todos.value))
}

const loadTodos = () => {
  const saved = localStorage.getItem('vue-todos')
  if (saved) {
    try {
      const parsed = JSON.parse(saved)
      todos.value = parsed.map((todo: any) => ({
        ...todo,
        createdAt: new Date(todo.createdAt)
      }))
    } catch (error) {
      console.error('Failed to load todos:', error)
    }
  }
}

onMounted(() => {
  loadTodos()
})

watch(todos, saveTodos, { deep: true })
</script>

<style scoped>
.todo-app {
  background: white;
  border-radius: 16px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  animation: slideIn 0.3s ease-out;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.header {
  background: linear-gradient(135deg, #3B82F6 0%, #1D4ED8 100%);
  color: white;
  padding: 32px 24px;
  text-align: center;
}

.title {
  font-size: 32px;
  font-weight: 700;
  margin-bottom: 8px;
  letter-spacing: -0.02em;
}

.subtitle {
  font-size: 16px;
  opacity: 0.9;
  font-weight: 400;
}

.todo-container {
  padding: 24px;
}

.stats-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 24px 0;
  padding: 0 8px;
}

.task-count {
  font-size: 14px;
  color: #6B7280;
  font-weight: 500;
}

.footer-actions {
  display: flex;
  justify-content: center;
  margin-top: 24px;
  padding-top: 16px;
  border-top: 1px solid #F3F4F6;
}

.clear-completed {
  background: #FEF2F2;
  color: #DC2626;
  border: 1px solid #FECACA;
  padding: 8px 16px;
  border-radius: 8px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}

.clear-completed:hover {
  background: #FEE2E2;
  border-color: #F87171;
  transform: translateY(-1px);
}

@media (max-width: 768px) {
  .header {
    padding: 24px 16px;
  }
  
  .title {
    font-size: 28px;
  }
  
  .todo-container {
    padding: 16px;
  }
  
  .stats-bar {
    flex-direction: column;
    gap: 16px;
    align-items: stretch;
  }
}
</style>