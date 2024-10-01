<template>
  <div class="bg-white shadow-md rounded-lg p-6 mb-8">
    <h2 class="text-2xl font-bold mb-4">{{ isEditing ? 'Edit Todo' : 'Add New Todo' }}</h2>
    <form @submit.prevent="handleSubmit">
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
      <div class="flex space-x-2">
        <button
          type="submit"
          class="flex-grow bg-blue-500 text-white py-2 px-4 rounded-lg hover:bg-blue-600 transition duration-200"
        >
          {{ isEditing ? 'Update Todo' : 'Add Todo' }}
        </button>
        <button
          type="button"
          @click="resetForm"
          class="bg-gray-300 text-gray-700 py-2 px-4 rounded-lg hover:bg-gray-400 transition duration-200"
        >
          Clear
        </button>
      </div>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue'
import { Todo } from '../types'

export default defineComponent({
  props: {
    editingTodo: {
      type: Object as () => Todo | null,
      default: null
    }
  },
  emits: ['add-todo', 'update-todo'],
  setup(props, { emit }) {
    const title = ref('')
    const todo = ref('')
    const expiry_date = ref('')
    const isEditing = ref(false)

    const resetForm = () => {
      title.value = ''
      todo.value = ''
      expiry_date.value = ''
      isEditing.value = false
    }

    const handleSubmit = () => {
      const todoData = {
        title: title.value,
        todo: todo.value,
        expiry_date: expiry_date.value || null
      }

      if (isEditing.value && props.editingTodo) {
        emit('update-todo', { ...todoData, id: props.editingTodo.id })
      } else {
        emit('add-todo', todoData)
      }

      resetForm()
    }

    watch(
      () => props.editingTodo,
      (newValue) => {
        if (newValue) {
          title.value = newValue.title
          todo.value = newValue.todo
          expiry_date.value = newValue.expiry_date || ''
          isEditing.value = true
        } else {
          resetForm()
        }
      },
      { immediate: true }
    )

    return {
      title,
      todo,
      expiry_date,
      isEditing,
      handleSubmit,
      resetForm
    }
  }
})
</script>
