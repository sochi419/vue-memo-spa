<script>
export default {
  data() {
    return {
      storageKey: 'todoList',
      newTodo: '',
      todoList: []
    }
  },
  methods: {
    add() {
      if (this.newTodo.trim() === '') {
        return
      }
      for (let i = 0; i < this.todoList.length; i++) {
        this.todoList[i].done = false
      }
      this.todoList.push({ text: this.newTodo, editText: '' })
      localStorage.setItem(this.storageKey, JSON.stringify(this.todoList))
      this.newTodo = ''
    },
    remove(index) {
      this.todoList.splice(index, 1)
      localStorage.setItem(this.storageKey, JSON.stringify(this.todoList))
    },
    update(index) {
      const todo = this.todoList[index]
      if (todo.editText !== '') {
        todo.text = todo.editText
        todo.editText = ''
        localStorage.setItem(this.storageKey, JSON.stringify(this.todoList))
      }
    },
    setEditTodoValue(todo, index) {
      this.todoList[index].editText = todo.text
    }
  },
  created() {
    const dataStr = localStorage.getItem(this.storageKey)
    if (dataStr) {
      this.todoList = JSON.parse(dataStr)
      for (let i = 0; i < this.todoList.length; i++) {
        this.todoList[i].done = false
      }
    }
  }
}
</script>

<template>
  <main>
    <div id="app">
      <textarea
        id="message"
        name="message"
        cols="30"
        rows="7"
        v-model="newTodo"
        placeholder="memo内容を入力してください "
      ></textarea>
      <button @click="add">追加</button>

      <ul>
        <li v-for="(todo, index) in todoList" :key="index">
          <label>
            <input type="checkbox" v-model="todo.done" @change="setEditTodoValue(todo, index)" />
            <span @click="todo.done = !todo.done">{{ todo.text }}</span>
          </label>
          <input type="text" v-show="todo.done" v-model="todo.editText" />
          <button @click="update(index)" v-show="todo.done">編集</button>
          <button @click="remove(index)" v-show="todo.done">削除</button>
        </li>
      </ul>
    </div>
  </main>
</template>
