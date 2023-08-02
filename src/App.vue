<script setup>

import { ref } from 'vue'

const todos = ref([
  { text: "Placeholder To-do Item", id: crypto.randomUUID(), done:false },
  { text: "Blah blah blah", id: crypto.randomUUID(), done:false },
])

const inputText = ref(null)

// Adding
function addTodo() {
  todos.value.push({ text: inputText.value, id: crypto.randomUUID(), done:false })
  inputText.value = ""
}

// Editing
const editText = ref(null)
const editing = ref(null)
function editTodo(todo) {
  editText.value = todo.text
  editing.value = todo
}
function saveEdit(todo) {
  todo.text = editText.value
  editing.value = null
  editText.value = null
}

// Removing
function removeTodo(todo) {
  todos.value = todos.value.filter((i) => i != todo)
}

</script>


<template>
  <div class="wrapper">
    <div class="list">
      <div class="todo" v-for="todo in todos" :key="todo.id" :class="todo.done ? 'completed':''">
        <div v-if="editing != todo">{{ todo.text }}</div>
        <div v-else><input type="text" v-model="editText" class="editField" /></div>
        <div>
          <!-- Editing -->
          <button v-if="editing != todo" @click="editTodo(todo)">E</button>
          <button v-else @click="saveEdit(todo)" class="saveBtn">S</button>
          <!-- Removing -->
          <button @click="removeTodo(todo)">X</button>
          <!-- Completing -->
          <button @click="todo.done = !todo.done">C</button>
        </div>
      </div>
    </div>
    <div class="input">
      <input type="text" v-model="inputText" /><button @click="addTodo">Add</button>
    </div>
  </div>
</template>


<style>
body {
  padding: 0;
  margin: 0;
  width: 100vw;
  height: 100vh;
  background-color: cadetblue;
  display: grid;
  align-items: center;
  justify-content: center;
}

.wrapper {
  background-color: #f2f2f2;
  padding: 20px;
  border-radius: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.todo {
  width: 600px;
  margin: 0 0 10px;
  padding: 10px;
  box-sizing: border-box;
  border-radius: 10px;
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
  background-color: white;
  display: flex;
  justify-content: space-between;
  align-items: center;
  text-align: left;
}

.editField {
  background-color: #f2f2f2;
  padding: 5px;
  box-sizing: border-box;
  font-size: 1rem;
  border-radius: 5px;
  border: none;
  margin-right: 10px;
  width: 450px;
}

.todo button {
  color: white;
  background-color: wheat;
  width: 30px;
  height: 30px;
  padding: 0;
  transition: 0.5s;
  border-radius: 10px;
  cursor: pointer;
  border: none;
  margin-left: 8px;
}

.todo button:hover {
  background-color: lightsalmon;
  transition: 0.5s;
}

.todo .saveBtn {
  background-color: lightblue;
}

.todo .saveBtn:hover {
  background-color: lightseagreen;
}


.input {
  display: flex;
  justify-content: space-between;
}

.input input {
  height: 50px;
  padding: 10px;
  box-sizing: border-box;
  font-size: 1rem;
  border-radius: 10px;
  border: 1px solid lightgrey;
  width: 100%;
  margin-right: 10px;
}

.input button {
  color: white;
  background: lightblue;
  width: 60px;
  height: 50px;
  padding: 0;
  transition: 0.5s;
  border-radius: 10px;
  cursor: pointer;
  border: none;
}

.input button:hover {
  background: lightseagreen;
  transition: 0.5s;
}

.completed{
  background-color:lightyellow;
  text-decoration:line-through;
}
</style>