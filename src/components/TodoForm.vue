<template>
     <div>
    <input 
      v-model="newTodo" 
      @keyup.enter="addTodo" 
      placeholder="Add a new todo..." 
      style="padding: 10px; font-size: 16px; border: 1px solid #ccc; border-radius: 5px; width: 50%;"
    />
  </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        newTodo: '',
      };
    },
    methods: {
      addTodo() {
        if (this.newTodo.trim() === '') return;
        this.$emit('add', this.newTodo);
        this.newTodo = '';
      },
    },
  };
  </script>
  