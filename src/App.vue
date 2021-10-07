<template>
  <div id="app">
    <AddingTodo/>
   
  </div>
</template>

<script>

import AddingTodo from './components/AddingTodo.vue'

export default {
  name: 'App',
  components: {
    AddingTodo
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
