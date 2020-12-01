<template>
   <div>
    <h1>Wonderlist</h1>
    <h2>With this list you can add, edit, check complete and delete task that you need to do.
    Start now organizing yourself.</h2>
    <md-card>
      <md-field>
        <md-input
          v-model="currentTodo"
          @keydown.enter="addTodo()"
          placeholder="This is what I have to take care of..."
        ></md-input>
      </md-field>
      <ul class="todos">
        <li class="todo-item" v-for="todo in todos" :key="todo.id">
          <input
            class="completed"
            type="checkbox"
            @change="togglecompleted"
            v-model="todo.completed"
          />
          <span
            v-if="todo.editing === false"
            class="editing"
            :class="{ editing: todo == editedTodo }"
            @dblclick="editTodo(todo)"
          >
            {{ todo.label }}
          </span>
          <md-field v-if="todo.editing" class="editing">
            <md-input
              type="text"
              v-model="todo.label"
              @keyup.enter="stopEdit(todo)"
              @keyup.esc="stopEdit(todo)"
              @focusout="stopEdit(todo)"
              placeholder="Add item here"
            />
          </md-field>
          <button v-if="todo.editing === false" @click="editTodo(todo)">
            Edit
          </button>
          <button v-if="todo.editing === true" @click="stopEdit(todo)">
            Save
          </button>
          <button v-if="todo.editing === false" @click="removeTodo(todo)">
            Delete
          </button>
          <button v-if="todo.editing === true" @click="editTodo(todo)">
            Cancel
          </button>
        </li>
      </ul>
    </md-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      currentTodo: '',
      editedTodo: ''
    };
  },
  methods: {
    addTodo() {
      this.todos.push({id: this.todos.length, label: this.currentTodo, completed: false, editing: false});
      this.currentTodo = '';
    },
    removeTodo(todo) {
      var index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    },
    togglecompleted() {
      this.todos.completed = !this.todos.completed;
    },
    editTodo(todo) {
      const index = this.todos.indexOf(todo);
      this.todos[index].editing = !this.todos[index].editing;
    },
    stopEdit(todo) {
      todo.editing = false;
    }
  }
};
</script>

<style>
body {
  margin: 5% 10% !important;
  background-color: lightblue;
}

ul {
  list-style-type: none;
}
.md-field .md-input,
.md-field .md-textarea {
  background-color: whitesmoke !important;
}

</style>