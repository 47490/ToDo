<script setup>
import { ref, onMounted } from 'vue';

const item = ref('');
const list = ref([]);

onMounted(() => {
  if (localStorage.getItem('tasks')) {
    list.value = JSON.parse(localStorage.getItem('tasks'));
  }
});

function addItem() {
  let listItem = { 'text': item.value, 'done': false };
  list.value.push(listItem);
  item.value = '';
  saveTasks();
}

function radera(listItem) {
  list.value = list.value.filter((obj) => {
    return obj !== listItem;
  });
  saveTasks();
}

function toggleDone(task) {
  task.done = !task.done;
  saveTasks();
}

function saveTasks() {
  localStorage.setItem('tasks', JSON.stringify(list.value));
}
</script>


<template>
  <header>
    <h1>tasklist</h1>
  </header>

  <main>
    <p>
      task: <input type="text" v-model="item" placeholder="type the task here">
      <button v-on:click="addItem()">Add</button>
    </p>
    <h3 id="todo">ToDo...</h3>
      <ul>
        <template v-for="itm in list">
          <li v-if="!itm.done" v-bind:key="itm" class="todoItm" @click="toggleDone(itm)">
            {{ itm.text }} <span title="delete" @click="radera(itm)">X</span>
          </li>

        </template>
      </ul>
    <h3 id="klart">done</h3>
      <ul>
        <template v-for="itm in list">
          <li v-if="itm.done" v-bind:key="itm" class="todoItm done" @click="toggleDone(itm)">
            {{ itm.text }} <span title="delete" @click="radera(itm)">X</span>
          </li>

        </template>
      </ul>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}
h1{
  font-weight: bold;
  text-align: center;
  color: purple;
}
li span{
  display: inline-block;
  position: absolute;
  right: 3em;
  color: blueviolet;
  padding-left:  0.5em;
  padding-right: 0.5em;
}
button {

  color:purple;
}
#todo{
  font-weight: bold;
  background-color: rgb(82, 1, 1);
  color: red;
  text-align: center;
}
.todoItm {
  margin-left: 2em;
  font-weight: bold;
  background-color: rgb(63, 0, 37);
  padding: 0.5em;
  list-style-type: none;
  cursor: pointer;
}
.todoItm:nth-child(odd) {
  background-color:rgb(223, 168, 255);
}
.todoItm:nth-child(even) {
  background-color: rgb(169, 158, 255);
}
.done{
  
  text-decoration: line-through;
}
.done:nth-child(odd) {
  background-color:rgb(226, 255, 168);
  color: rgb(0, 97, 58);
}
.done:nth-child(even) {
  background-color: rgb(158, 255, 195);
  color: rgb(11, 0, 97);
}
#klart {

  font-weight: bold;
  background-color: rgb(106, 255, 0);
  text-align: center;
  color: green;
}
</style>
