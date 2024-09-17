<template>
  <div :class="darkMode ? 'dark' : 'light'">
    <input v-model="newTodo" placeholder="Nueva tarea" />
    <button @click="addTodo">Agregar</button>

    <div>
      <button @click="filter = 'all'">Todas</button>
      <button @click="filter = 'active'">Activas</button>
      <button @click="filter = 'completed'">Completadas</button>
    </div>

    <ul class="todo-list">
      <li v-for="(todo, index) in filteredTodos" :key="todo.id" class="todo-item">
        <div class="todo-content">
          <input type="checkbox" v-model="todo.completed" />
          <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
        </div>
        <button @click="deleteTodo(index)" class="delete-btn">Eliminar</button>
      </li>
    </ul>

    <button @click="clearCompleted" class="clear-btn">Eliminar Completadas</button>
    <button @click="toggleDarkMode" class="toggle-btn">Cambiar a {{ darkMode ? 'Modo Claro' : 'Modo Oscuro' }}</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      filter: 'all',
      darkMode: false
    };
  },
  computed: {
    filteredTodos() {
      if (this.filter === 'active') {
        return this.todos.filter(todo => !todo.completed);
      } else if (this.filter === 'completed') {
        return this.todos.filter(todo => todo.completed);
      } else {
        return this.todos;
      }
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() !== '') {
        this.todos.push({ id: Date.now(), text: this.newTodo, completed: false });
        this.newTodo = '';
      }
    },
    deleteTodo(index) {
      this.todos.splice(index, 1);
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);
    },
    toggleDarkMode() {
      this.darkMode = !this.darkMode;
    }
  }
};
</script>

<style>
.todo-list {
  list-style-type: none;
  margin: 0;
  padding: 0;
  width: 100%;
}

.todo-item {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  width: 100%;
}

.todo-content {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-grow: 1;
}

.completed {
  text-decoration: line-through;
  color: gray;
}

.delete-btn {
  margin-left: 10px;
  flex-shrink: 0;
}

button:hover {
  background-color: #f8b8d4;
  color: white;
  cursor: pointer;
}

.todo-item:hover span {
  color: #f8b8d4;
}

.delete-btn:hover {
  background-color: #f8b8d4;
  color: white;
  cursor: pointer;
}

.light {
  background-color: white;
  color: black;
}

.dark {
  background-color: black;
  color: white;
}

.clear-btn, .toggle-btn {
  width: auto;
  margin-top: 20px;
  padding: 10px 20px;
}

@media (max-width: 600px) {
  .todo-list {
    width: 90%;
  }

  .clear-btn, .toggle-btn {
    font-size: 1rem;
  }
}
</style>