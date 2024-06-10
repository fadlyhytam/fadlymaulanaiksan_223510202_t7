<template>
  <div>
    <h1>Todo List</h1>
    <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a new task" />
    <ul>
      <li v-for="(todo, index) in todos" :key="index" class="todo-item">
        <span :class="{ completed: todo.completed }" @click="toggleTodoCompletion(index)">
          <span class="checkmark" v-if="todo.completed">✔</span>
          {{ todo.text }}
        </span>
        <button @click="removeTodo(index)">Delete</button>
      </li>
    </ul>
    <p>{{ incompleteTodosCount }} tasks remaining</p>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import { useTodoStore } from '../stores/todoStore'

export default {
  setup() {
    const newTodo = ref('')
    const todoStore = useTodoStore()

    const addTodo = () => {
      if (newTodo.value.trim()) {
        todoStore.addTodo(newTodo.value.trim())
        newTodo.value = ''
      }
    }

    const removeTodo = (index) => {
      todoStore.removeTodo(index)
    }

    const toggleTodoCompletion = (index) => {
      todoStore.toggleTodoCompletion(index)
    }

    const incompleteTodosCount = computed(() => {
      return todoStore.todos.filter((todo) => !todo.completed).length
    })

    return {
      newTodo,
      todos: todoStore.todos,
      addTodo,
      removeTodo,
      toggleTodoCompletion,
      incompleteTodosCount
    }
  }
}
</script>

<style scoped>
h1 {
  font-size: 2rem;
  margin-bottom: 1rem;
  color: #3a3a3a;
}

input {
  width: 100%;
  padding: 10px;
  margin-bottom: 1rem;
  border: 2px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
}

ul {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

.todo-item:last-child {
  border-bottom: none;
}

.todo-item span {
  cursor: pointer;
  flex-grow: 1;
  display: flex;
  align-items: center;
}

.todo-item span.completed {
  text-decoration: line-through;
  color: #aaa;
}

.todo-item .checkmark {
  margin-right: 10px;
  color: green;
  font-weight: bold;
  font-size: 1.2rem;
}

button {
  background-color: #e74c3c;
  border: none;
  color: white;
  padding: 5px 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 14px;
  margin: 2px 1px;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #c0392b;
}

p {
  margin-top: 1rem;
  font-size: 1rem;
  color: #666;
}
</style>
