<script setup>
import { ref } from 'vue';

// TODO入力欄の値
const todoRef = ref('');
// 編集モード
const isEditRef = ref(false); // true:編集モード false:追加モード

// ローカルストレージ保存用配列
const todoListRef = ref([]);
const ls = localStorage.todoList;
// ローカルストレージにtodoListRefが存在していればparseし、
// なければundifinedになるため空配列をセットする
todoListRef.value = ls ? JSON.parse(ls) : [];

// 追加ボタン押下時
const addTodo = () => {
  // IDをミリ秒単位で登録
  const id = new Date().getTime();

  // 配列に入力TODOを格納
  todoListRef.value.push({ id: id, task: todoRef.value });

  // ローカルストレージに登録
  // localStorage.todoList.push(JSON.stringify(todoRef.value));
  localStorage.todoList = JSON.stringify(todoListRef.value);

  // 登録後は入力欄を空にする
  todoRef.value = '';
};

// 編集対象インデックス参照用
let editId = -1;

// 編ボタン押下時
const showTodo = (id) => {
  //  todoListRefから引数のidと同じ要素を検索
  // 「(todo)」には配列の要素が入っている
  const todo = todoListRef.value.find((todo) => todo.id === id);
  todoRef.value = todo.task; // 取得した要素からtaskを取り出す
  isEditRef.value = true;
  editId = id;
};

// 削除ボタン押下時
const deleteTodo = (id) => {
  // 削除対象のTODOを取得
  const todo = todoListRef.value.find((todo) => todo.id === id);
  // TODOリストから編集対象のインデックスを取得
  const idx = todoListRef.value.findIndex((todo) => todo.id === editId);

  const delMsg = '「' + todo.task + '」を削除しますか？';

  if (!confirm(delMsg)) return;

  todoListRef.value.splice(idx, 1);
};

// 変更ボタン押下時
const editTodo = () => {
  // 編集対象となるTODOを取得
  const todo = todoListRef.value.find((todo) => todo.id === editId);

  // TODOリストから編集対象のインデックスを取得
  const idx = todoListRef.value.findIndex((todo) => todo.id === editId);

  // taskを編集後のTODOで置き換え
  todo.task = todoRef.value;

  //インデックスを元に対象オブジェクトを置き換え
  todoListRef.value.splice(idx, 1, todo);

  // ローカルストレージに保存
  localStorage.todoList = JSON.stringify(todoListRef.value);

  // 編集モード解除
  isEditRef.value = false;
  // IDを初期値に戻す
  editId = -1;
  todoRef.value = '';
};
</script>

<template>
  <div class="box_input">
    <input
      type="text"
      class="todo_input"
      v-model="todoRef"
      placeholder="＋ TODOを入力"
    />
    <button class="btn green" @click="editTodo" v-show="isEditRef">変更</button>
    <button class="btn" @click="addTodo" v-show="!isEditRef">追加</button>
  </div>
  <div class="box_list">
    <div class="todo_list" v-for="todo in todoListRef" :key="todo.id">
      <div class="todo">
        <input type="checkbox" class="check" />
        <label>{{ todo.task }}</label>
      </div>
      <div class="btns">
        <button class="btn green" @click="showTodo(todo.id)">編</button>
        <button class="btn pink" @click="deleteTodo(todo.id)">削</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.box_input {
  margin-top: 20px;
}
.todo_input {
  width: 300px;
  margin-right: 8px;
  padding: 8px;
  font-size: 18px;
  border: lpx solid #aaa;
  border-radius: 6px;
}
.btn {
  padding: 8px;
  background-color: #03a9f4;
  border-radius: 6px;
  color: #fff;
  text-align: center;
  font-size: 14px;
}
.box_list {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}
.todo_list {
  display: flex;
  align-items: center;
  gap: 8px;
}
.todo {
  border: 1px solid #ccc;
  border-radius: 6px;
  padding: 12px;
  width: 300px;
}
.check {
  border: 1px solid red;
  transform: scale(1.6);
  margin: 0 16px 2px 6px;
}
.btns {
  display: flex;
  gap: 4px;
}
.green {
  background-color: #00c853;
}
.pink {
  background-color: #ff4081;
}
</style>
