<template>
  <div class="bg-white shadow-md rounded-lg p-6">
    <h2 class="text-2xl font-bold mb-4">Todo List</h2>
    <div v-if="todos.length === 0" class="text-gray-500 text-center">
      No todos yet. Add one to get started!
    </div>
    <ul v-else>
      <li v-for="todo in todos" :key="todo.id" class="mb-4 p-4 border rounded-lg">
        <div class="flex items-start justify-between">
          <div class="flex-grow pr-4">
            <h3 class="text-lg font-semibold" :class="{ 'line-through': todo.completed }">
              {{ todo.title }}
            </h3>
            <p class="text-gray-600" :class="{ 'line-through': todo.completed }">
              {{ todo.todo }}
            </p>
            <p v-if="todo.expiry_date" class="text-sm text-gray-500">
              Expires: {{ new Date(todo.expiry_date).toLocaleString() }}
            </p>
          </div>
          <div class="flex-shrink-0 space-x-2 whitespace-nowrap">
            <button
              @click="$emit('edit', todo)"
              class="bg-yellow-500 text-white py-1 px-2 rounded hover:bg-yellow-600"
            >
              Edit
            </button>
            <button
              @click="$emit('toggle-complete', todo.id)"
              class="bg-green-500 text-white py-1 px-2 rounded hover:bg-green-600"
            >
              {{ todo.completed ? 'Undo' : 'Complete' }}
            </button>
            <button
              @click="$emit('delete', todo.id)"
              class="bg-red-500 text-white py-1 px-2 rounded hover:bg-red-600"
            >
              Delete
            </button>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue'
import { Todo } from '../types'

export default defineComponent({
  props: {
    todos: {
      type: Array as PropType<Todo[]>,
      required: true
    }
  },
  emits: ['toggle-complete', 'delete', 'edit']
})
</script>
