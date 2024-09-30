<template>
  <form @submit.prevent="handleSubmit" class="mb-8">
    <div class="mb-4">
      <label for="title" class="block text-gray-700 font-bold mb-2">Title</label>
      <input
        type="text"
        id="title"
        v-model="title"
        required
        class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
      />
    </div>
    <div class="mb-4">
      <label for="todo" class="block text-gray-700 font-bold mb-2">Description</label>
      <textarea
        id="todo"
        v-model="todo"
        required
        class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
      ></textarea>
    </div>
    <div class="mb-4">
      <label for="expiry_date" class="block text-gray-700 font-bold mb-2"
        >Expiry Date (Optional)</label
      >
      <input
        type="datetime-local"
        id="expiry_date"
        v-model="expiry_date"
        class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
      />
    </div>
    <button
      type="submit"
      class="w-full bg-blue-500 text-white py-2 px-4 rounded-lg hover:bg-blue-600 transition duration-200"
    >
      Add Todo
    </button>
  </form>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  emits: ['add-todo'],
  setup(_, { emit }) {
    const title = ref('')
    const todo = ref('')
    const expiry_date = ref('')

    const handleSubmit = () => {
      emit('add-todo', {
        title: title.value,
        todo: todo.value,
        expiry_date: expiry_date.value || null
      })
      title.value = ''
      todo.value = ''
      expiry_date.value = ''
    }

    return {
      title,
      todo,
      expiry_date,
      handleSubmit
    }
  }
})
</script>
