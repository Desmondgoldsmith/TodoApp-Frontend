<template>
  <div>
    <h2 class="text-2xl font-bold mb-4">Todo List</h2>
    <TodoItem
      v-for="todo in todos"
      :key="todo.id"
      :todo="todo"
      @toggle-complete="$emit('toggle-complete', $event)"
      @delete="$emit('delete', $event)"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue'
import { Todo } from '../types'
import TodoItem from './TodoItem.vue'

export default defineComponent({
  components: {
    TodoItem
  },
  props: {
    todos: {
      type: Array as PropType<Todo[]>,
      required: true
    }
  },
  emits: ['toggle-complete', 'delete']
})
</script>
