
<template>
  <div id="app">
    <TodoForm @add="addTodo" />
    <transition-group name="fade" tag="ul" class="todo-list">
      <li v-for="todo in activeTodos" :key="todo.id">
        <TodoItem :todo="todo" @toggle="toggleTodo" @save="saveTodo" @remove="removeTodo" />
      </li>
    </transition-group>
    <h2 class="completed-header">Completed Todos</h2>
    <transition-group name="fade" tag="ul" class="completed-list">
      <li v-for="todo in completedTodos" :key="todo.id">
        {{ todo.text }} (Created: {{ formatDate(todo.created_at) }})
      </li>
    </transition-group>
  </div>
</template>

<script>
import TodoForm from './components/TodoForm.vue';
import TodoItem from './components/TodoItem.vue';


export default {
  components: {
    TodoForm,
    TodoItem,
   
  },
  data() {
    return {
      todos: [],
    };
  },
  computed: {
    activeTodos() {
      return this.todos.filter(todo => !todo.completed);
    },
    completedTodos() {
      return this.todos.filter(todo => todo.completed);
    },
  },
  methods: {
    addTodo(text) {
      this.todos.push({
        id: Date.now(),
        text: text,
        completed: false,
        created_at: new Date(),
      });
    },
    toggleTodo(todo) {
      const index = this.todos.findIndex(item => item.id === todo.id);
      if (index !== -1) {
        this.todos[index].completed = todo.completed;
      }
    },
    saveTodo(updatedTodo) {
      const index = this.todos.findIndex(item => item.id === updatedTodo.id);
      if (index !== -1) {
        this.todos[index].text = updatedTodo.text;
      }
    },
    removeTodo(todoToRemove) {
      this.todos = this.todos.filter(todo => todo.id !== todoToRemove.id);
    },
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' };
      return new Date(date).toLocaleDateString(undefined, options);
    },
    loadTodosFromLocalStorage() {
      const savedTodos = localStorage.getItem('todos');
      if (savedTodos) {
        this.todos = JSON.parse(savedTodos);
      }
    },
  },
  mounted() {
    this.loadTodosFromLocalStorage();

    window.addEventListener('beforeunload', () => {
      localStorage.setItem('todos', JSON.stringify(this.todos));
    });
  },
  beforeUnmount() {
    localStorage.setItem('todos', JSON.stringify(this.todos));
  },
};
</script>

<style scoped>
@import './styles.css';
</style>

