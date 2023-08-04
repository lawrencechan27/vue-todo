<script setup>
import { ref, onMounted } from 'vue';
import Navigation from './Navigation.vue'
import List from './List.vue'

// Define vars
const todos = ref([])
const lists = ref([])
const currentList = ref()

// Check if todos exist in local storage, and get them
function fetchTodos() {
    const savedLists = JSON.parse(localStorage.getItem("lists"))
    const savedTodos = JSON.parse(localStorage.getItem("todos"))
    if (savedLists) {
        lists.value = savedLists
        currentList.value = lists.value[0]
        todos.value = savedTodos
    } else {
        // If no local storage exists, start with a default "Tasks" list
        lists.value = ["Tasks"]
        currentList.value = lists.value[0]
    }
}

function saveToLocalStorage() {
    localStorage.setItem("lists", JSON.stringify(lists.value))
    localStorage.setItem("todos", JSON.stringify(todos.value))
}

onMounted(() => {
    fetchTodos();
})

// json-server mounting

// onMounted(() => {
//   fetch("http://localhost:3000/todos")
//     .then((res) => res.json())
//     .then((data) => todos.value = data)
//     .catch((err) => console.log(err.message))
// })

// Functions emitted from Navigation component
const changeList = (list) => {
    currentList.value = list
}
const addNewList = (newListName) => {
    lists.value.push(newListName)
    console.log(lists)
}
// Functions emitted from List component
const addTodo = (inputText) => {
    todos.value.push({ text: inputText, id: crypto.randomUUID(), done: false, list: currentList.value })
    console.table(todos.value)
    saveToLocalStorage()
}
const removeTodo = (todo) => {
    todos.value = todos.value.filter((i) => i != todo)
    saveToLocalStorage()
}
const saveEdit = (todo, editText) => {
    todo.text = editText
    saveToLocalStorage()
}
</script>



<template>
    <Navigation :lists="lists" :currentList="currentList" @change-list="changeList" @add-new-list="addNewList" />
    <List :currentList="currentList" :todos="todos" @add-todo="addTodo" @remove-todo="removeTodo" @save-edit="saveEdit" />
</template>



<style>
body {
    padding: 0;
    margin: 0;
    width: 100vw;
    height: 100vh;
    background-color: cadetblue;
    display: grid;
    align-items: top;
    justify-content: center;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
</style>