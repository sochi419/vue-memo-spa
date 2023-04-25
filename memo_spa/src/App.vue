<template>
  <div id="app">
    <div class="memo_list">
      <ul>
        <li v-for="(memo, index) in memoList" :key="index">
          <span @click=";(selectedMemo = memo), (addMemo = false), (editMemoValue = memo.text)">{{
            memo.text.split('\n')[0]
          }}</span>
        </li>
        <li><span @click=";(addMemo = !addMemo), (selectedMemo = null)">+</span></li>
      </ul>
    </div>

    <div class="addform" v-if="addMemo">
      <textarea
        cols="50"
        rows="10"
        v-model="newMemo"
        placeholder="memo内容を入力してください "
      ></textarea>
      <button @click="add">追加</button>
    </div>

    <div class="editform" v-if="selectedMemo !== null">
      <textarea cols="50" rows="10" v-model="editMemoValue"></textarea>
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
      addMemo: false,
      editMemoValue: ''
    }
  },
  methods: {
    add() {
      if (this.newMemo.trim() === '') {
        return
      }
      this.memoList.push({ text: this.newMemo })
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
      if (this.editMemoValue !== '') {
        memo.text = this.editMemoValue
        localStorage.setItem(this.storageKey, JSON.stringify(this.memoList))
        this.selectedMemo = null
      }
    }
  },
  created() {
    const dataStr = localStorage.getItem(this.storageKey)
    if (dataStr) {
      this.memoList = JSON.parse(dataStr)
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
