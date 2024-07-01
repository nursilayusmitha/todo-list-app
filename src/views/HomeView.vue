<script>
import { ref, computed } from 'vue'
import axios from 'axios'
import { url } from '../source.jsx'

export default {
  setup() {
    const todos = ref([])
    const searchQuery = ref('')

    const fetchTodos = async () => {
      try {
        const response = await axios.get(url)
        todos.value = response.data
      } catch (error) {
        console.error('Error fetching todos:', error)
      }
    }

    const toggleCompleted = (todo) => {
      todo.completed = !todo.completed
    }

    const filteredTodos = computed(() => {
      return todos.value.filter((todo) =>
        todo.title.toLowerCase().includes(searchQuery.value.toLowerCase())
      )
    })

    fetchTodos()

    return {
      todos,
      searchQuery,
      filteredTodos,
      toggleCompleted
    }
  }
}
</script>

<template>
  <div class="container">
    <div class="header">
      <div class="image-container">
        <img src="/public/todos.svg" alt="Animated Image" class="animated-image" />
      </div>
      <h1 class="title animated-title">To Do List</h1>
    </div>
    <div class="content">
      <div class="todo-container">
        <input v-model="searchQuery" placeholder="Search by title" />
        <table>
          <thead>
            <tr>
              <th>ID</th>
              <th>Title</th>
              <th>Completed</th>
              <th>Toggle</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="todo in filteredTodos"
              :key="todo.id"
              :class="{ 'completed-yes': todo.completed }"
            >
              <td>{{ todo.id }}</td>
              <td :class="{ 'completed-title': todo.completed }">{{ todo.title }}</td>
              <td>{{ todo.completed ? 'Yes' : 'No' }}</td>
              <td>
                <input type="checkbox" v-model="todo.completed" />
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0 auto;
  max-width: 800px;
}

.header {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.title {
  font-size: 2.5em;
  color: #34495e;
  margin-left: 20px;
}

.animated-title {
  animation: bounce 2s infinite;
}

@keyframes bounce {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

.content {
  display: flex;
  justify-content: space-between;
  width: 100%;
}

.image-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.animated-image {
  width: 150px;
  height: 150px;
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%,
  100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-20px);
  }
}

.todo-container {
  flex: 2;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

input {
  margin-bottom: 10px;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  background-color: #34495e;
  color: #ecf0f1;
  border: 1px solid #bdc3c7;
}

input::placeholder {
  color: #95a5a6;
}

table {
  width: 100%;
  border-collapse: collapse;
  color: #2c3e50;
}

th,
td {
  border: 1px solid #bdc3c7;
  padding: 8px;
  text-align: left;
}

thead {
  background-color: #34495e;
  color: #ecf0f1;
}

tbody tr:hover {
  background-color: #ecf0f1;
}

tbody tr.completed-yes {
  background-color: #d5dbdb;
}

.completed-title {
  text-decoration: line-through;
}
</style>
