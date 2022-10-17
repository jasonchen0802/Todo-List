<template>
  <div id = "app">
    <div class="newTodo">
      <input
        class="input"
        type="text"
        v-model="input"
        placeholder="write something to do..."
      >
      <button
        class="createBtn"
        :disabled="!input"
        @click="addTodo()">
        Add
      </button>
    </div>
    <n-card
      v-for = "(Todo, index) in Todos"
      :key="index"
      :class="[Todo.done ? 'cardDecoration' : null]">
      <div class="column">
        <input type="checkbox" class="checkBox" v-model = "Todo.done">
        <div
          class="content"
          :class="[Todo.done ? 'contentDecoration' : null]"
          contenteditable>
          {{Todo.content}}
        </div>
        <n-button
          class="complete"
          srtong secondary type="success"
          @click="editTodo(index)">
          Save
        </n-button>
        <n-button
          class="delete"
          strong secondary type="error"
          @click="deleteTodo(index)">
          &#x2716;
        </n-button>
      </div>
    </n-card>
  </div>
</template>

<script>
import { NButton, NCard } from 'naive-ui'
export default {
  components: {
    NButton,
    NCard
  },
  data () {
    return {
      input: '',
      Todos: []
    }
  },

  mounted () {
    this.getTodos()
  },
  renderTriggered () {
    this.setTodo()
  },
  methods: {
    setTodo () {
      localStorage.setItem('Todo', JSON.stringify(this.Todos))
    },
    getTodos () {
      this.Todos = JSON.parse(localStorage.getItem('Todo'))
    },
    addTodo () {
      this.Todos.unshift({
        content: this.input,
        done: false
      })
      this.input = ''
    },
    editTodo (index) {
      this.Todos[index].content = document.getElementsByClassName('content')[index].innerHTML
      this.setTodo()
      alert('已更新!')
    },
    completeTodo (index) {
      this.Todos[index].done = true
    },
    deleteTodo (index) {
      this.Todos.splice(index, 1)
    }
  }
}
</script>

<style scoped>
.newTodo {
  display: flex;
  margin:100px auto 40px;
  max-width: 600px;
}
.input {
  font-size: 20px;
  margin-right: 20px;
  border: 0;
  outline: none;
  width: 500px;
  border-radius: 10px;
  padding: 10px;
}
.createBtn {
  width: 10%;
  padding: 10px;
  font-weight: bold;
  font-size: 18px;
  border: 0px;
  border-radius: 10px;
  transition-duration: 0.1s;
  color: white;
  background-color: #2080f0;
}
.createBtn:hover {
  background-color: #4da0ff;
}
.createBtn:active {
  background-color: #0059be;
}
.createBtn:disabled {
  transition-duration: 0.1s;
  background-color: #cccccc;
}
.n-card {
  display: flex;
  border-radius: 10px;
  margin: 20px auto;
  width: 600px;
  transition-duration: 0.1s;
}
.cardDecoration {
  background-color: #defcec;
}
.column {
  display: flex;
}
.checkBox {
  margin-right: 5px;
  width: 25px;
}
.checkBox:hover {
  background-color: #0059be;
}
.content {
  margin-right: 10px;
  width: 100%;
  font-size: 18px;
  padding: 5px;
  outline: none;
}
.contentDecoration {
  text-decoration: line-through;
  color: #a1a1a1;
}
.complete, .delete{
  margin-right: 10px;
  padding: 0 15px;
}
</style>
