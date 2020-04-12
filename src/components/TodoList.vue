<template>
  <div>
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    
    <div v-for="(todo, index) in todosFiltered" :key="todo.id">
        <div class="todo-item">
            <div class="todo-item-left">
                <input type="checkbox" v-model="todo.isCompleted">
                <div 
                    v-if="!todo.isEditing" 
                    @dblclick="editTodo(todo)" 
                    class="todo-item-label"
                    :class="{ completed: todo.isCompleted}"
                    >
                    {{ todo.title }}
                </div>
                <input v-else class="todo-item-edit" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" type="text" v-model="todo.title" v-focus>
            </div>
            <div class="remove-item" @click="removeTodo(index)">
                &times;
            </div>
        </div>
    </div>


    <div class="extra-container">
        <div>
            <label>
                <input type="checkbox" @change="checkAllTodo($event)">
                    Check All
            </label>
        </div>
        <div>{{ remaining }} items left</div>
    </div>

    <div class="extra-container">
        <div>
            <button :class="{ active: filter == 'all' }" @click="filter = 'all'" >All</button>
            <button :class="{ active: filter == 'active' }" @click="filter = 'active'" >Active</button>
            <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'" >Completed</button>
        </div>
        <div>
           <transition name="fade">
                <button v-if="showClearCompletedButton" @click="clearCompleted">
                    Clear Completed
                </button>
           </transition>
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
          filter: 'all',
          todos: [
              {
                  'id': 1,
                  'title': 'Finish Vue Screencast',
                  'isCompleted': false,
                  'isEditing': false,
              },
              {
                  'id': 2,
                  'title': 'Take over world',
                  'isCompleted': false,
                  'isEditing': false
              },
          ],
          beforeEditCache :'',
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
              isCompleted: false,
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
      },
      checkAllTodo(event){
          return this.todos.map((todo) => todo.isCompleted = event.target.checked);
      },
      clearCompleted(){
          this.todos = this.todos.filter((todo) => todo.isCompleted == false);
      }
  },
  directives: {
        focus: {
            // directive definition
            inserted: function (el) {
                el.focus()
            }
        }
  },
  computed: {
      remaining(){
          return this.todos.filter(todo => todo.isCompleted == false).length;
      },
      todosFiltered(){
          if(this.filter == 'all'){
              return this.todos.filter((todo) => todo.isCompleted == true || todo.isCompleted == false )
          }
          else if(this.filter == 'active'){
            return this.todos.filter((todo) => todo.isCompleted == false)
          }
          else if(this.filter == 'completed'){
            return this.todos.filter((todo) => todo.isCompleted == true)
          }

          return this.todos;
      },
      showClearCompletedButton(){
          return this.todos.filter((todo) => todo.isCompleted == true).length > 0;
      },
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

    .completed{
        text-decoration: line-through;
        color: grey;
    }

    .extra-container{
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px !important;
        border-top: 1px solid lightgrey;
        padding-top: 14px;
        margin-bottom: 14px;

        div, label{
            font-size: 16px !important;
        }
    }

    button{
        font-size: 14px;
        background-color: white;
        appearance: none;

        &:hover{
            background: lightgreen;
        }

        &:focus{
            outline: none;
        }
    }

    .active{
        background: lightgreen;
    }

    .fade-enter-active, .fade-leave-active{
        transition: opacity .2s;
    }

    .fade-enter, .fade-leave-to{
        opacity: 0
    }

</style>
