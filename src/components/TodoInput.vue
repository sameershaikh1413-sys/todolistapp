<template>
  <form @submit.prevent="handleSubmit" class="todo-input-form">
    <div class="input-container">
      <input
        ref="inputRef"
        v-model="inputText"
        type="text"
        placeholder="What needs to be done?"
        class="todo-input"
        maxlength="200"
      />
      <button 
        type="submit" 
        class="add-button"
        :disabled="!inputText.trim()"
      >
        <PlusIcon />
      </button>
    </div>
  </form>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import PlusIcon from './icons/PlusIcon.vue'

const emit = defineEmits<{
  addTodo: [text: string]
}>()

const inputText = ref('')
const inputRef = ref<HTMLInputElement | null>(null)

const handleSubmit = () => {
  const text = inputText.value.trim()
  if (text) {
    emit('addTodo', text)
    inputText.value = ''
  }
}

onMounted(() => {
  inputRef.value?.focus()
})
</script>

<style scoped>
.todo-input-form {
  margin-bottom: 8px;
}

.input-container {
  position: relative;
  display: flex;
  align-items: center;
  background: #F9FAFB;
  border: 2px solid #E5E7EB;
  border-radius: 12px;
  transition: all 0.2s ease;
}

.input-container:focus-within {
  border-color: #3B82F6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
  background: white;
}

.todo-input {
  flex: 1;
  border: none;
  background: transparent;
  padding: 16px 20px;
  font-size: 16px;
  color: #111827;
  outline: none;
  border-radius: 12px;
}

.todo-input::placeholder {
  color: #9CA3AF;
}

.add-button {
  background: #3B82F6;
  color: white;
  border: none;
  width: 48px;
  height: 48px;
  border-radius: 8px;
  margin: 4px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
  flex-shrink: 0;
}

.add-button:hover:not(:disabled) {
  background: #2563EB;
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
}

.add-button:disabled {
  background: #D1D5DB;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

@media (max-width: 768px) {
  .todo-input {
    padding: 14px 16px;
    font-size: 16px; /* Prevents zoom on iOS */
  }
  
  .add-button {
    width: 44px;
    height: 44px;
  }
}
</style>