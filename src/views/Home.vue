<template>
  <div id="app">

    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"></Todos>
    <AddTodo v-on:add-todo="addTodo"></AddTodo>
  </div>
</template>

<script>
import Todos from '../components/Todos';

import AddTodo from '../components/AddTodo';
import axios from 'axios';

// Note using jsonplace holder to do fake JSON request stuff.
// https://jsonplaceholder.typicode.com/guide/
export default {
  name: 'Home',
  components: {
    Todos, AddTodo
  },
  data() {
    return {
      todos: [
      ]
    }
  },
    methods: {
      deleteTodo(id) {
        axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
          .then(() => {
            this.todos = this.todos.filter(todo => todo.id !== id)
          })
          .catch((e) => console.log(e));
      },
      addTodo(newTodo) {
        const { title, completed } = newTodo;
        // will return to us the new json object with new id using the data we submitted.
        // fake server response
        axios.post('https://jsonplaceholder.typicode.com/todos', {
          title,
          completed
        })
          .then((res) => this.todos = [ ...this.todos, res.data] )
          .catch((e) => console.log(e));
      }
  },
  created() {
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
      .then((res) => this.todos = res.data)
      .catch((e) => console.log(e));
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
.btn:hover {
  background: #666;
}
</style>

