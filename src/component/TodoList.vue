<template class="all">
  <body>
  <div>
    <!-- title -->
    <h1 class="title">Time Manager</h1>

      <!-- Input todo to create todo-->
    <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTask" @keyup.enter="add">

    <div class="extra-container">
      <!-- buttons, For todo active completed and all -->
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
        <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</button>
        <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</button>
      </div>

      <!-- Animation fade when user create and delete todo-->
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
      <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="todo-item">
        <div class="todo-item-left">
          <!-- Check box -->
          <input type="checkbox" v-model="todo.completed">

          <!-- Edit Todo by double-clicking -->
          <div v-if="!todo.editing" @dblclick="edit(todo)" class="todo-item-label" :class="{ completed : todo.completed }">{{ todo.title }}</div>
          <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
        </div>
        <!--To remove a to-do X-->
        <div class="done" @click="remove(index)">
          &times;
        </div>
      </div>
    </transition-group>

    <div class="extra-container">
      <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"> Check All</label></div>
      <div>{{ remain }} items left</div>
    </div>


      <!-- Button to clear completed -->
      <div>
        <transition name="fade">
          <button class="done" v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
        </transition>
      </div>

    </div>
  </div>

</body>


</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      newTask: '',
      idForTodo: 3,
      beforeEditCache: '',
      filter: 'all',
      todos: [
        {
          'id': 1,
          'title': 'Finish this project! T-T',
          'completed': false,
          'editing': false,
        },
        {
          'id': 2,
          'title': 'Cry in bed for 5 mins',
          'completed': false,
          'editing': false,
        },
      ]
    }
  },
  // Find what is remaining
  computed: {
    remain() {
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining() {
      return this.remain != 0
    },
    todosFiltered() {
      if (this.filter == 'all') {
        return this.todos
      } else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.completed)
      }

      return this.todos
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  methods: {
    add() {
      if (this.newTask.trim().length == 0) {
        return
      }

      this.todos.push({
        id: this.idForTodo,
        title: this.newTask,
        completed: false,
        editing: false,
      })

      this.newTask = ''
      this.idForTodo++
    },
    edit(todo) {
      this.beforeEditCache = todo.title
      todo.editing = true
    },
    doneEdit(todo) {
      if (todo.title.trim() == '') {
        todo.title = this.beforeEditCache
      }
      todo.editing = false
    },
    cancelEdit(todo) {
      todo.title = this.beforeEditCache
      todo.editing = false
    },
    remove(index) {
      this.todos.splice(index, 1)
    },
    checkAllTodos() {
      this.todos.forEach((todo) => todo.completed = event.target.checked)
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
}
</script>


<style>

.title {
  position: relative;
  padding: 0;
  margin: 0;
  font-family: "Raleway", sans-serif;
  font-weight: 300;
  font-size: 40px;
  color: #080808;
  -webkit-transition: all 0.4s ease 0s;
  -o-transition: all 0.4s ease 0s;
  transition: all 0.4s ease 0s;
}

button {
   background-color: rgba(51, 51, 51, 0.05);
   border-radius: 8px;
   border-width: 0;
   color: #333333;
   cursor: pointer;
   display: inline-block;
   font-family: "Haas Grot Text R Web", "Helvetica Neue", Helvetica, Arial, sans-serif;
   font-size: 14px;
   font-weight: 500;
   line-height: 20px;
   list-style: none;
   margin: 0;
   padding: 10px 12px;
   text-align: center;
   transition: all 200ms;
   vertical-align: baseline;
   white-space: nowrap;
   user-select: none;
   -webkit-user-select: none;
   touch-action: manipulation;

}

button:hover {
  background-color: green;
  color: white;
}

.all {
  max-width: 500px;
  margin: auto;
  position: relative;
}

.extra-container {
  position: relative;
  padding: 0;
  margin: 0;
  font-family: "Raleway", sans-serif;
  font-weight: 300;
  font-size: 19px;
  color: #080808;
  -webkit-transition: all 0.4s ease 0s;
  -o-transition: all 0.4s ease 0s;
  transition: all 0.4s ease 0s;
}

.done {
  box-shadow: inset #ffffff;
  background-color: #ffffff;
  border-radius: 6px;
  border: 1px solid #dcdcdc;
  display: inline-block;
  cursor: pointer;
  color: #666666;
  font-size: 15px;
  font-weight: bold;
  padding: 6px 24px;
  text-decoration: none;
}

.done:hover {
  background-color: green;
  color: white;
}

.todo-input {
  display: flex;
  width: 1000px;
  padding: 10px 18px;
  font-size: 18px;
  max-width: 500px;
  margin: auto;
}

.todo-item {
  display: flex;
  width: 1000px;
  padding: 10px 18px;
  font-size: 18px;
  max-width: 500px;
  margin: auto;
}

.todo-item-edit {
  display: flex;
  width: 1000px;
  padding: 10px 18px;
  font-size: 18px;
  max-width: 500px;
  margin: auto;

}

ol {
  list-style-type: none;
  font-weight: bold;
  font-size: 20px;
}

ul {
  list-style-type: none;
  font-weight: bold;
  font-size: 20px;
}



</style>