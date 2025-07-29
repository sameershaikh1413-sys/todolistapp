<template>
  <div class="todo-filters">
    <button
      v-for="filter in filters"
      :key="filter.value"
      :class="['filter-button', { active: currentFilter === filter.value }]"
      @click="emit('filterChange', filter.value)"
    >
      {{ filter.label }}
    </button>
  </div>
</template>

<script setup lang="ts">
import type { FilterType } from '../types/todo'

defineProps<{
  currentFilter: FilterType
}>()

const emit = defineEmits<{
  filterChange: [filter: FilterType]
}>()

const filters = [
  { value: 'all' as FilterType, label: 'All' },
  { value: 'active' as FilterType, label: 'Active' },
  { value: 'completed' as FilterType, label: 'Completed' }
]
</script>

<style scoped>
.todo-filters {
  display: flex;
  gap: 4px;
  background: #F3F4F6;
  padding: 4px;
  border-radius: 8px;
}

.filter-button {
  padding: 8px 16px;
  border: none;
  background: transparent;
  color: #6B7280;
  font-size: 14px;
  font-weight: 500;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.filter-button:hover {
  color: #374151;
  background: #E5E7EB;
}

.filter-button.active {
  background: white;
  color: #3B82F6;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

@media (max-width: 768px) {
  .todo-filters {
    width: 100%;
  }
  
  .filter-button {
    flex: 1;
    text-align: center;
  }
}
</style>