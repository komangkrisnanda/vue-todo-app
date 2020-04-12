<template>
  <div>
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    
    <div v-for="(todo, index) in todos" :key="todo.id">
        <div class="todo-item">
            {{ todo.title }}
            <div class="remove-item" @click="removeTodo(index)">
                &times;
            </div>
        </div>
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

          // check if todo empty or not

          if(this.newTodo.trim().length == 0){
              return;
          }

          this.todos.push({
              id: this.idForTodo,
              title: this.newTodo,
              completed: false
          });

          this.idForTodo++;
          this.newTodo = '';
      },
      removeTodo(index){
          this.todos.splice(index, 1);
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
    
    .todo-item{
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .remove-item{
        cursor: pointer;
        margin-left: 14px;
        &:hover{
            color: black;
        }
    }
</style>
