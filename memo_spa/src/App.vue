<script>
export default {
  data() {
    return {
      storageKey: 'memoList',
      newMemo: '',
      memoList: []
    }
  },
  methods: {
    add() {
      if (this.newMemo.trim() === '') {
        return
      }
      for (let i = 0; i < this.memoList.length; i++) {
        this.memoList[i].done = false
      }
      this.memoList.push({ text: this.newMemo, editText: '' })
      localStorage.setItem(this.storageKey, JSON.stringify(this.memoList))
      this.newMemo = ''
    },
    remove(index) {
      this.memoList.splice(index, 1)
      localStorage.setItem(this.storageKey, JSON.stringify(this.memoList))
    },
    update(index) {
      const memo = this.memoList[index]
      if (memo.editText !== '') {
        memo.text = memo.editText
        memo.editText = ''
        localStorage.setItem(this.storageKey, JSON.stringify(this.memoList))
      }
    },
    setEditMemoValue(memo, index) {
      this.memoList[index].editText = memo.text
    }
  },
  created() {
    const dataStr = localStorage.getItem(this.storageKey)
    if (dataStr) {
      this.memoList = JSON.parse(dataStr)
      for (let i = 0; i < this.memoList.length; i++) {
        this.memoList[i].done = false
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
        v-model="newMemo"
        placeholder="memo内容を入力してください "
      ></textarea>
      <button @click="add">追加</button>

      <ul>
        <li v-for="(memo, index) in memoList" :key="index">
          <label>
            <input type="checkbox" v-model="memo.done" @change="setEditMemoValue(memo, index)" />
            <span @click="memo.done = !memo.done">{{ memo.text.split('\n')[0] }}</span>
          </label>
          <input type="text" v-show="memo.done" v-model="memo.editText" />
          <button @click="update(index)" v-show="memo.done">編集</button>
          <button @click="remove(index)" v-show="memo.done">削除</button>
        </li>
      </ul>
    </div>
  </main>
</template>
