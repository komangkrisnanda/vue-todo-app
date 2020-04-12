<template>
  <div>
      <input type="text" class="todo-input" placeholder="What needs to be done">
      Todo List goes Here
  </div>
</template>

<script>

export default {
  name: 'todo-list',
}
</script>

<style lang="scss">
    .todo-input{
        width: 100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;

        &:focus{
            outline: 0;
        }
    }
</style>
