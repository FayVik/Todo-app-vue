<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="AddTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Header from "./components/layout/Header";
import Todos from "./components/Todos";
import AddTodo from "./components/AddTodo";

export default {
  name: "app",
  components: {
    Header,
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    AddTodo(newTodo) {
      const { title, completed } = newTodo;

      fetch("https://jsonplaceholder.typicode.com/todos", {
        method: "POST",
        body: {
          title,
          completed
        }
      })
        .then(response => response.json())
        .then(data => (this.todos = [...this.todos, data]))
        .catch(err => err);
    }
  },
  created() {
    fetch("https://jsonplaceholder.typicode.com/todos?_limit=5")
      .then(response => response.json())
      .then(data => (this.todos = data))
      .catch(err => err);
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;

  padding: 0;
}
body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}
.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 5px 20px;
  cursor: pointer;
}
.btn:hover {
  background: #666;
}
</style>
