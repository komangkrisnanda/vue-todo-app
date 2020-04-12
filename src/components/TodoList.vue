<template>
  <div>
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    
    Todo List goes Here

    <div v-for="todo in todos" :key="todo.id">
        <p>{{ todo.title }}</p>
    </div>

  </div>
</template>

<script>

export default {
  name: 'todo-list',
  data(){
      return {
          idForTodo: 3,
          newTodo: '',
          todos: [
              {
                  'id': 1,
                  'title': 'Finish Vue Screencast',
                  'completed': false,
              },
              {
                  'id': 2,
                  'title': 'Take over world',
                  'completed': false
              },
          ]
      }
  },
  methods: {
      addTodo(){
          this.todos.push({
              id: this.idForTodo,
              title: this.newTodo,
              completed: false
          });

          this.idForTodo++;
          this.newTodo = '';
      }
  }
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
