<script>
export default {
  data: function () {
    return {
      storageKey: 'todolist',
      newtodo: '',
      todolist: []
    }
  },

  methods: {
    addTodo: function () {
      this.todolist.push({ text: this.newtodo })
      localStorage.setItem(this.storageKey, JSON.stringify(this.todolist))
      this.newtodo = ''
    },
    remove: function (index) {
      this.todolist.splice(index, 1)
      localStorage.setItem(this.storageKey, JSON.stringify(this.todolist))
    },
    update: function (index) {
      this.todolist.splice(index, 1, { text: this.newtodo })
      localStorage.setItem(this.storageKey, JSON.stringify(this.todolist))
      this.newtodo = ''
    }
  },

  created() {
    var dataStr = localStorage.getItem(this.storageKey)
    if (dataStr) {
      this.todolist = JSON.parse(dataStr)
    }
  },

  todolist: function () {
    localStorage.setItem(this.storageKey, JSON.stringify(this.todolist))
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
        v-model="newtodo"
        placeholder="memo内容を入力してください "
      ></textarea>
      <button @click="addTodo">追加</button>

      <ul>
        <li v-for="(todo, index) in todolist">
          <input type="checkbox" v-model="todo.done" />
          <span>{{ todo.text.split(/\n/)[0] }}</span>
        </li>
      </ul>
    </div>
  </main>

  <div>
    <div v-for="(todo, index) in todolist">
      <div id="app" v-if="todo.done">
        <textarea
          id="message"
          name="message"
          cols="30"
          rows="7"
          type="text"
          v-model="newtodo"
          placeholder="編集内容を入力してください "
          v-show="todo.done"
        ></textarea
        ><br /><button @click="update(index)" v-show="todo.done">編集</button><br /><button
          @click="remove(index)"
          v-show="todo.done"
        >
          削除
        </button>
      </div>
    </div>
  </div>
</template>
