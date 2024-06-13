<script>
import { ref } from 'vue';

export default {
  name: 'App',
  setup() {
    const newTodo = ref('');
    const defaultData = [{
      done: false,
      content: 'Write a blog post'
    }]
    const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData;
    const todos = ref(todosData);

    function addTodo() {
      if (newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value
        });
        newTodo.value = '';
      }
      saveData();
    }

    function doneTodo(todo) {
      todo.done = !todo.done;
      saveData();
    }

    function removeTodo(index) {
      todos.value.splice(index, 1);
      saveData();
    }

    function saveData() {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem('todos', storageData);
    }

    return {
      todos,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      saveData
    }
  }
}
</script>

<template>
  <div class="container col-4">
    <h1 class="my-4 text-center">ToDo App</h1>
    <form @submit.prevent="addTodo" class="form-inline justify-content-center mb-4">
      <div class="row  align-items-center">
        <div class="col-3">
          <label for="newTodo" class="col-form-label">New ToDo</label>
        </div>
        <div class="col-7">
          <input v-model="newTodo" name="newTodo" id="newTodo" class="form-control" placeholder="Enter new todo"
            autocomplete="off" />
        </div>
        <div class="col-2">
          <button class="btn btn-primary">Add</button>
        </div>
      </div>
    </form>
    <h2 class="my-4 text-center">ToDo List</h2>
    <ul class="list-group">
      <li v-for="(todo, index) in todos" :key="index"
        class="list-group-item d-flex justify-content-between align-items-center" >
        <span :class="{ 'text-decoration-line-through': todo.done }" @click="doneTodo(todo)" role="button">
          {{ todo.content }}
        </span>
        <button class="btn btn-danger btn-sm" @click="removeTodo(index)">Remove</button>
      </li>
    </ul>
    <h4 v-if="todos.length === 0" class="mt-4 text-center">Empty list.</h4>
  </div>
</template>

<style>
.text-decoration-line-through {
  text-decoration: line-through;
}
</style>
