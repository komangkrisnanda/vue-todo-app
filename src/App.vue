<template>
  <div id="app" class="container">
    <div class="logo">
      <img alt="Vue logo" src="./assets/logo.png">
    </div>
    <todo-list></todo-list>
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue'


export default {
  name: 'App',
  components: {
    TodoList
  }
}
</script>

<style>
*{
  box-sizing: border-box;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-size: 24px;
}

.container{
  max-width: 600px;
  margin: 0 auto;
}

.logo{
  text-align: center;
  width: 100%;
}
</style>
