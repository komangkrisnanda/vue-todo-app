<template>
  <div>
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    
    <div v-for="(todo, index) in todos" :key="todo.id">
        <div class="todo-item">
            <div class="todo-item-left">
                <div v-if="!todo.isEditing" @dblclick="editTodo(todo)" class="todo-item-label">{{ todo.title }}</div>
                <input v-else class="todo-item-edit" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" type="text" v-model="todo.title" v-focus>
            </div>
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
                  'isEditing': false,
              },
              {
                  'id': 2,
                  'title': 'Take over world',
                  'completed': false,
                  'isEditing': false
              },
          ],
          beforeEditCache :''
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
              completed: false,
              isEditing: false
          });

          this.idForTodo++;
          this.newTodo = '';
      },
      removeTodo(index){
          this.todos.splice(index, 1);
      },
      editTodo(todo){
          this.beforeEditCache = todo.title;
          todo.isEditing = true;
      },
      doneEdit(todo){
          if(this.newTodo.trim().length == 0){
              this.cancelEdit(todo);
          }
          todo.isEditing = false;
      },
      cancelEdit(todo){
          todo.title = this.beforeEditCache;
          todo.isEditing = false;
      }
  },
  directives: {
        focus: {
            // directive definition
            inserted: function (el) {
                el.focus()
            }
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

    .todo-item-left{
        display: flex;
        align-items: center;
    }

    .todo-item-label{
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }

    .todo-item-edit{
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Helvetica, Arial, sans-serif;

        &:focus{
            outline: none;
        }
    }

</style>
