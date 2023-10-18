<template>
  <div>
    <input type="checkbox" v-model="todoCopy.completed" @change="toggleTodo" />
    <span v-if="!editing" :class="{ completed: todoCopy.completed }">{{ todoCopy.text }}</span>
    <input v-if="editing" v-model="editText" @keyup.enter="saveTodo" @blur="saveTodo" />
    <button v-if="!editing" @click="editTodo">Edit</button>
    <button v-if="editing" @click="cancelEdit">Cancel</button>
    <small>Created: {{ formatDate(todoCopy.created_at) }}</small>
    <button @click="removeTodo">Remove</button>
  </div>
</template>



<script>
export default {
  props: {
    todo: Object,
  },
  data() {
    return {
      todoCopy: { ...this.todo }, // Create a copy of the todo prop
      editing: false,
      editText: '',
    };
  },
  methods: {

    editTodo() {
      this.editing = true;
      this.editText = this.todoCopy.text;
    },
    cancelEdit() {
      this.editing = false;
      this.editText = this.todoCopy.text;
    },
    toggleTodo() {
      this.$emit('toggle', { ...this.todoCopy }); // Emit a copy of the todoCopy object
    },
   
    saveTodo() {
      if (this.editText.trim() === '') {
        this.removeTodo();
      } else {
        this.todoCopy.text = this.editText;
        this.editing = false;
        this.$emit('save', { ...this.todoCopy }); // Emit a copy of the todoCopy object after editing
      }
    },
    removeTodo() {
      this.$emit('remove', { ...this.todoCopy }); // Emit a copy of the todoCopy object for removal
    },
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' };
      return new Date(date).toLocaleDateString(undefined, options);
    },
  },
};
</script>

<style scoped>
.completed {
  text-decoration: line-through;
}
</style>
