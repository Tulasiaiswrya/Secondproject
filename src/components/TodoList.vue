<template>
  <div>
    <h2>Todo List</h2>
    <div v-if="todos.length === 0">No todos available.</div>
    <div v-else>
      <div v-for="todo in todos" :key="todo.id" class="todo-item" @mouseover="setHovered(todo.id)" @mouseleave="clearHovered">
        <TodoItem :todo="todo" @toggle="toggleTodo" @save="saveTodo" @remove="removeTodo" :isHovered="hoveredTodo === todo.id" />
        <button v-show="hoveredTodo === todo.id" class="delete-button" @click="removeTodoHandler(todo.id)">Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
import TodoItem from './TodoItem.vue';

export default {
  props: {
    todos: Array,
  },
  components: {
    TodoItem,
  },
  data() {
    return {
      hoveredTodo: null,
    };
  },
  methods: {
    toggleTodo(todo) {
      this.$emit('toggle', todo);
    },
    saveTodo(todo) {
      this.$emit('save', todo);
    },
    removeTodo(todo) {
      this.$emit('remove', todo);
    },
    setHovered(todoId) {
      this.hoveredTodo = todoId;
    },
    clearHovered() {
      this.hoveredTodo = null;
    },
    removeTodoHandler(todoId) {
      this.$emit('remove', { id: todoId });
    },
  },
};
</script>

<style scoped>
@import './src/styles.css';

</style>
