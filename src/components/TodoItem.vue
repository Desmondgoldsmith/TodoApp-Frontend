<template>
  <div class="flex items-center justify-between p-4 bg-white shadow-md rounded-lg mb-4">
    <div class="flex items-center">
      <input
        type="checkbox"
        :checked="todo.completed"
        @change="$emit('toggle-complete', todo.id)"
        class="mr-4 h-5 w-5 text-blue-600"
      />
      <div>
        <h3 class="text-lg font-semibold" :class="{ 'line-through': todo.completed }">
          {{ todo.title }}
        </h3>
        <p class="text-gray-600">{{ todo.todo }}</p>
        <p v-if="todo.expiry_date" class="text-sm text-red-500">
          Expires: {{ new Date(todo.expiry_date).toLocaleString() }}
        </p>
      </div>
    </div>
    <button @click="$emit('delete', todo.id)" class="text-red-500 hover:text-red-700">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-6 w-6"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M6 18L18 6M6 6l12 12"
        />
      </svg>
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue'
import { Todo } from '../types'

export default defineComponent({
  props: {
    todo: {
      type: Object as PropType<Todo>,
      required: true
    }
  }
})
</script>
