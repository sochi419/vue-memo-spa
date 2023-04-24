<template>
  <div id="app">
    <div class="memo_list">
      <ul>
        <li v-for="(memo, index) in memoList" :key="index">
          <span @click=";(selectedMemo = memo), (addMemo = false)">{{
            memo.text.split('\n')[0]
          }}</span>
        </li>
        <li><span @click=";(addMemo = !addMemo), (selectedMemo = null)">+</span></li>
      </ul>
    </div>

    <div class="addform" v-if="addMemo">
      <textarea
        id="message"
        name="message"
        cols="50"
        rows="10"
        v-model="newMemo"
        placeholder="memo内容を入力してください "
      ></textarea>
      <button @click="add">追加</button>
    </div>

    <div class="editform" v-if="selectedMemo !== null">
      <textarea
        id="message"
        name="message"
        cols="50"
        rows="10"
        v-model="selectedMemo.editText"
        placeholder="memo内容を入力してください "
      ></textarea>
      <button @click="update(memoList.indexOf(selectedMemo))">更新</button>
      <button @click="remove(memoList.indexOf(selectedMemo))">削除</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      storageKey: 'memoList',
      newMemo: '',
      memoList: [],
      selectedMemo: null,
      addMemo: false
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
      this.selectedMemo = null
    },
    update(index) {
      const memo = this.memoList[index]
      if (memo.editText !== '') {
        memo.text = memo.editText
        memo.editText = ''
        localStorage.setItem(this.storageKey, JSON.stringify(this.memoList))
        this.selectedMemo = null
      }
    }
  },
  watch: {
    selectedMemo(newMemo) {
      if (newMemo !== null) {
        newMemo.editText = newMemo.text
      }
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

<style>
.memo_list {
  position: fixed;
  left: 300px;
  top: 250px;
}

.editform {
  left: 500px;
}

.addform {
  left: 500px;
}

li:hover {
  cursor: pointer;
}

li:hover span {
  color: blue;
}
</style>
