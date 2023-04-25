<template>
  <div id="app">
    <div id = "contentWrap">
      <div class="nowDateSection">
        {{ nowDate }}
        <input type="text" ref="todoContent" @keydown.enter="addTodoList" maxlength="30" value="test"/>
      </div>
      <ul>
        <li v-for="val in todoList" :key="val.state" class="todoList" :idx="val.idx">
          <input type="checkbox" @click="changeState(val.idx)" :checked="val.state == 'done' ? true : false"/>
          <span :class="{'finish' : val.state == 'done'}">
            {{ val.todo }}
          </span>
          <button @click="removeTodoList(val.idx)">X</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue';
import { TodoListType } from "./type/index";

const todoListIdx = ref<number>(3);
const nowDate = ref(new Date().getFullYear() + '년 ' + ( new Date().getMonth() + 1 ) + '월 ' + new Date().getDate() + '일');

const todoList = ref<TodoListType[]>([
  {idx : 0, state : 'before', todo : '기가막히게 멋지게 집에 가기ㅎㅎ'},
  {idx : 1, state : 'done', todo : '대단하고 야무지게 밥먹깅'},
  {idx : 2, state : 'before', todo : '스르륵 잠에 들기'},
]);
const todoList2 = reactive<TodoListType[]>([
  {idx : 0, state : 'before', todo : '기가막히게 멋지게 집에 가기ㅎㅎ'},
  {idx : 1, state : 'done', todo : '대단하고 야무지게 밥먹깅'},
  {idx : 2, state : 'before', todo : '스르륵 잠에 들기'},
]);

const changeState = (idx: number) => {
  todoList.value.map((v)=> v.state =  v.idx == idx ? v.state === 'before' ? 'done' : 'before' : v.state);
};
const addTodoList = (e) => {
  todoList.value.push({ idx : todoListIdx, state : 'before', todo: e.target.value });
  todoListIdx.value++;
  e.target.value = '';
};
const removeTodoList = (idx: number) => {
  todoList.value = todoList.value.filter((v)=>v.idx != idx);
};
</script>

<style>
@font-face {
  font-family: 'TheJamsil5Bold';
  src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_2302_01@1.0/TheJamsil5Bold.woff2') format('woff2');
  font-weight: 300;
  font-style: normal;
}
#app * {
  font-family: 'TheJamsil5Bold';
}
#app {
  display: flex;
  justify-content: center;
  align-items: center;
}

#contentWrap {
  width : 490px;
  height : auto;
  margin-top: 200px;
}
.nowDateSection {
  width : 490px;
  height : 70px;
  padding: 10px;
  text-align: center;
  font-size: larger;
  background-color: #FCC8D1;
  color: grey;
  margin-bottom: 10px;
  border-radius: 10px;
}
.nowDateSection input {
  width : 440px;
  height : 28px;
  font-size: large;
  margin-top: 5px;
}
.nowDateSection input:focus {
  outline: none;
}

ul {
  margin-block-start: 0em;
  margin-block-end: 0em;
  padding-inline-start: 0px;
}
.todoList{
  list-style-type: none;
  display: flex;
  margin-bottom: 4px;
}
.todoList input[type=checkbox]{
  margin-right: 10px;
}
.todoList span{
  list-style-type: none;
  width: 440px;
}

.finish{
  text-decoration: line-through;
  color: darkgray;
}
</style>
