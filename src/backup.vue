<template>
  <Header />
  <AddTodo v-on:add-todo="addTodo" />
  <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />  
</template>

<script>
import Header from './components/layout/Header'
import Todos from './components/Todos'
import AddTodo from './components/AddTodo'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    Header,
    Todos,
    AddTodo
  },
  data(){
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      //this.todos = this.todos.filter(todo => todo.id !== id)
      axios.delete('http://jsonplaceholder.typicode.com/todos/${id}')
      .then(res => {
        this.todos = this.todos.filter(todo => todo.id !== id)
        return res
      })
      .catch(error => {
        return Promise.reject(error)
      })
    },
    addTodo(newTodo) {
      const {title, completed} = newTodo
      axios.post('http://jsonplaceholder.typicode.com/todos',{
        title,
        completed
      }).then(res => this.todos = [...this.todos, res.data])
      .catch(error => {
        return Promise.reject(error)
      })
    }
  },
  created() {
    axios.get('http://jsonplaceholder.typicode.com/todos?_limit=5')
    .then(res => this.todos = res.data)
    .catch(error => {
      return Promise.reject(error)
    })
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;

}
body{
  font-family: Arial, Helvetica, sans-serif;
}
.btn{
  display: inline;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}
.btn:hover{ 
  background: #666;
}
</style>
