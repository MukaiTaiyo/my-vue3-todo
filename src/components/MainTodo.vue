<script setup lang="ts">
import { ref } from 'vue';

const todo = ref('');
const todoList = ref<{ id: number; task: string }[]>([]);

const ls = localStorage.todoList;

todoList.value = ls ? JSON.parse(ls) : [];

// addTodo内をすべて変更
const addTodo = () => {
  // IDを簡易的にミリ秒で登録する
  const id = new Date().getTime();

  // 配列に入力TODOを格納
  todoList.value.push({ id: id, task: todo.value });

  // ローカルストレージに登録
  localStorage.todoList = JSON.stringify(todoList.value);

  // 登録後は入力欄を空にする
  todo.value = '';
};

const isEdit = ref(false);

const showTodo = (id: number) => {
  // TODOリストからIDに一致するTODOを取得
  const findTodo = todoList.value.find((todo) => todo.id === id);

  // 取得した要素からtaskを取り出し、入力欄へ
  if (findTodo) {
    todo.value = findTodo.task;
    isEdit.value = true;
  }
};

const editTodo = () => {};
</script>

<template>
  <div>
    <input type="text" class="todo_input" v-model="todo" placeholder="+ TODOを入力" />
    <button class="btn green" @click="editTodo? v-if="isEdit">変更</button>
    <button class="btn" @click="addTodo" v-show="!isEdit">追加</button>
  </div>
  <div class="box_list">
    <div class="todo_list" v-for="todo in todoList" :key="todo.id">
      <div class="todo">
        <input type="checkbox" class="check" />
        <label>{{ todo.task }}</label>
      </div>
      <div class="btns">
        <button class="btn green" @click="showTodo(todo.id)">編</button>
        <button class="btn green" @click="editTodo">変更</button>
        <button class="btn" @click="addTodo">追加</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.box_list {
  display: flex;
  flex-direction: column;
  gap: 4px;
  justify-content: center;
  margin-top: 20px;
}

.todo_list {
  display: flex;
  gap: 8px;
  align-items: center;
}

.todo {
  width: 250px;
  padding: 6px 8px;
  border: 1px solid #ccc;
  border-radius: 6px;
}

.check {
  margin-right: 12px;
  transform: scale(1.6);
}

.btns {
  display: flex;
  gap: 4px;
}

.btn.green {
  background-color: #00c853;
}

.btn.pink {
  background-color: #ff4081;
}
.todo_input {
  width: 250px;
  padding: 6px 8px;
  margin-right: 8px;
  font-size: 18px;
  border: 1px solid #aaa;
  border-radius: 6px;
}

.btn {
  position: relative;
  padding: 6px 8px;
  font-size: 14px;
  color: #fff;
  text-align: center;
  background-color: #03a9f4;
  border: 1px solid #eee;
  border-radius: 6px;
}

.btn:active {
  top: 1px;
}
</style>
