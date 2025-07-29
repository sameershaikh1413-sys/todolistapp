<template>
  <div class="todo-list">
    <TransitionGroup name="todo" tag="ul">
      <TodoItem
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @toggle="emit('toggleTodo', $event)"
        @delete="emit('deleteTodo', $event)"
        @edit="emit('editTodo', $event.id, $event.text)"
      />
    </TransitionGroup>
    
    <div v-if="todos.length === 0" class="empty-state">
      <div class="empty-icon">✨</div>
      <h3 class="empty-title">All caught up!</h3>
      <p class="empty-message">You have no tasks. Time to add some goals!</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import TodoItem from './TodoItem.vue'
import type { Todo } from '../types/todo'

defineProps<{
  todos: Todo[]
}>()

const emit = defineEmits<{
  toggleTodo: [id: number]
  deleteTodo: [id: number]
  editTodo: [id: number, text: string]
}>()
</script>

<style scoped>
.todo-list {
  min-height: 200px;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.empty-state {
  text-align: center;
  padding: 48px 24px;
  color: #6B7280;
}

.empty-icon {
  font-size: 48px;
  margin-bottom: 16px;
}

.empty-title {
  font-size: 20px;
  font-weight: 600;
  margin-bottom: 8px;
  color: #374151;
}

.empty-message {
  font-size: 16px;
  line-height: 1.5;
}

.todo-enter-active {
  transition: all 0.3s ease;
}

.todo-leave-active {
  transition: all 0.3s ease;
}

.todo-enter-from {
  opacity: 0;
  transform: translateX(-30px);
}

.todo-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.todo-move {
  transition: transform 0.3s ease;
}

@media (max-width: 768px) {
  .empty-state {
    padding: 32px 16px;
  }
  
  .empty-icon {
    font-size: 40px;
  }
  
  .empty-title {
    font-size: 18px;
  }
  
  .empty-message {
    font-size: 14px;
  }
}
</style>