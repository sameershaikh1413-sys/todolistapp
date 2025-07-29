<template>
  <li class="todo-item" :class="{ completed: todo.completed }">
    <div class="todo-content">
      <button 
        class="checkbox"
        :class="{ checked: todo.completed }"
        @click="emit('toggle', todo.id)"
      >
        <CheckIcon v-if="todo.completed" />
      </button>
      
      <div class="todo-text-container" v-if="!isEditing">
        <span 
          class="todo-text"
          @dblclick="startEditing"
        >
          {{ todo.text }}
        </span>
      </div>
      
      <div class="edit-container" v-else>
        <input
          ref="editInputRef"
          v-model="editText"
          @keyup.enter="saveEdit"
          @keyup.escape="cancelEdit"
          @blur="saveEdit"
          class="edit-input"
        />
      </div>
    </div>
    
    <div class="todo-actions">
      <button 
        class="action-button edit-button"
        @click="startEditing"
        v-if="!isEditing && !todo.completed"
        title="Edit task"
      >
        <EditIcon />
      </button>
      
      <button 
        class="action-button delete-button"
        @click="emit('delete', todo.id)"
        title="Delete task"
      >
        <TrashIcon />
      </button>
    </div>
  </li>
</template>

<script setup lang="ts">
import { ref, nextTick } from 'vue'
import type { Todo } from '../types/todo'
import CheckIcon from './icons/CheckIcon.vue'
import EditIcon from './icons/EditIcon.vue'
import TrashIcon from './icons/TrashIcon.vue'

const props = defineProps<{
  todo: Todo
}>()

const emit = defineEmits<{
  toggle: [id: number]
  delete: [id: number]
  edit: [event: { id: number; text: string }]
}>()

const isEditing = ref(false)
const editText = ref('')
const editInputRef = ref<HTMLInputElement | null>(null)

const startEditing = async () => {
  isEditing.value = true
  editText.value = props.todo.text
  await nextTick()
  editInputRef.value?.focus()
  editInputRef.value?.select()
}

const saveEdit = () => {
  const trimmedText = editText.value.trim()
  if (trimmedText && trimmedText !== props.todo.text) {
    emit('edit', { id: props.todo.id, text: trimmedText })
  }
  isEditing.value = false
}

const cancelEdit = () => {
  isEditing.value = false
  editText.value = props.todo.text
}
</script>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px;
  border: 1px solid #F3F4F6;
  border-radius: 12px;
  margin-bottom: 8px;
  background: white;
  transition: all 0.2s ease;
  group: todo-item;
}

.todo-item:hover {
  border-color: #E5E7EB;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  transform: translateY(-1px);
}

.todo-item.completed {
  background: #F9FAFB;
  opacity: 0.7;
}

.todo-content {
  display: flex;
  align-items: center;
  flex: 1;
  gap: 12px;
}

.checkbox {
  width: 24px;
  height: 24px;
  border: 2px solid #D1D5DB;
  border-radius: 6px;
  background: white;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s ease;
  flex-shrink: 0;
}

.checkbox:hover {
  border-color: #3B82F6;
}

.checkbox.checked {
  background: #3B82F6;
  border-color: #3B82F6;
  color: white;
}

.todo-text-container {
  flex: 1;
}

.todo-text {
  font-size: 16px;
  line-height: 1.5;
  color: #111827;
  cursor: pointer;
  word-break: break-word;
}

.completed .todo-text {
  text-decoration: line-through;
  color: #9CA3AF;
}

.edit-container {
  flex: 1;
}

.edit-input {
  width: 100%;
  border: 2px solid #3B82F6;
  border-radius: 6px;
  padding: 8px 12px;
  font-size: 16px;
  background: white;
  outline: none;
}

.todo-actions {
  display: flex;
  gap: 8px;
  opacity: 0;
  transition: opacity 0.2s ease;
}

.todo-item:hover .todo-actions {
  opacity: 1;
}

.action-button {
  width: 32px;
  height: 32px;
  border: none;
  border-radius: 6px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s ease;
}

.edit-button {
  background: #F3F4F6;
  color: #6B7280;
}

.edit-button:hover {
  background: #E5E7EB;
  color: #374151;
}

.delete-button {
  background: #FEF2F2;
  color: #DC2626;
}

.delete-button:hover {
  background: #FEE2E2;
  color: #B91C1C;
  transform: scale(1.05);
}

@media (max-width: 768px) {
  .todo-item {
    padding: 12px;
  }
  
  .todo-text {
    font-size: 15px;
  }
  
  .todo-actions {
    opacity: 1;
  }
  
  .action-button {
    width: 36px;
    height: 36px;
  }
}
</style>