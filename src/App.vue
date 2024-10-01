<template>
  <div class="container mx-auto px-4 py-8">
    <h1 class="text-4xl font-bold mb-8 text-center text-blue-600">Todo App</h1>
    <div class="grid md:grid-cols-2 gap-8">
      <TodoForm @add-todo="addTodo" @update-todo="updateTodo" :editing-todo="editingTodo" />
      <TodoList
        :todos="todos"
        @toggle-complete="toggleComplete"
        @delete="deleteTodo"
        @edit="editTodo"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue'
import TodoForm from './components/TodoForm.vue'
import TodoList from './components/TodoList.vue'
import { Todo } from './types'
import axios from 'axios'

export default defineComponent({
  components: {
    TodoForm,
    TodoList
  },
  setup() {
    const todos = ref<Todo[]>([])
    const editingTodo = ref<Todo | null>(null)

    const fetchTodos = async () => {
      try {
        const response = await axios.get('http://127.0.0.1:8000/todos')
        todos.value = response.data
      } catch (error) {
        console.error('Error fetching todos:', error)
      }
    }

    const addTodo = async (newTodo: Omit<Todo, 'id' | 'date_created'>) => {
      try {
        const response = await axios.post('http://127.0.0.1:8000/create_todo', newTodo)
        todos.value.push(response.data)
      } catch (error) {
        console.error('Error adding todo:', error)
      }
    }

    const updateTodo = async (updatedTodo: Todo) => {
      try {
        const response = await axios.put(
          `http://127.0.0.1:8000/update_todo/${updatedTodo.id}`,
          updatedTodo
        )
        const index = todos.value.findIndex((t) => t.id === updatedTodo.id)
        if (index !== -1) {
          todos.value[index] = response.data
        }
        editingTodo.value = null
      } catch (error) {
        console.error('Error updating todo:', error)
      }
    }

    const toggleComplete = async (id: number) => {
      const todo = todos.value.find((t) => t.id === id)
      if (todo) {
        try {
          const response = await axios.put(`http://127.0.0.1:8000/update_todo/${id}`, {
            ...todo,
            completed: !todo.completed
          })
          const index = todos.value.findIndex((t) => t.id === id)
          if (index !== -1) {
            todos.value[index] = response.data
          }
        } catch (error) {
          console.error('Error updating todo:', error)
        }
      }
    }

    const deleteTodo = async (id: number) => {
      try {
        await axios.delete(`http://127.0.0.1:8000/delete_todo/${id}`)
        todos.value = todos.value.filter((t) => t.id !== id)
      } catch (error) {
        console.error('Error deleting todo:', error)
      }
    }

    const editTodo = (todo: Todo) => {
      editingTodo.value = { ...todo }
    }

    onMounted(fetchTodos)

    return {
      todos,
      editingTodo,
      addTodo,
      updateTodo,
      toggleComplete,
      deleteTodo,
      editTodo
    }
  }
})
</script>
