<script setup>
import { ref } from 'vue';
import Navigation from './Navigation.vue'
import List from './List.vue'

// Array of to-do lists
const lists = ref(["Tasks", "Groceries"])

// Active list. Set to first in array as default
const currentList = ref(lists.value[0])

// Todo array. Written here for testing, but to be moved to DB later
const todos = ref([
    { text: "Pay bills", id: "crypto.randomUUID()", done: false, list: "Tasks" },
    { text: "Take out trash", id: "crypto.randomUUID()", done: false, list: "Tasks" },
    { text: "Bread", id: "crypto.randomUUID()", done: false, list: "Groceries" },
    { text: "Milk", id: "crypto.randomUUID()", done: false, list: "Groceries" },
    { text: "Eggs", id: "crypto.randomUUID()", done: false, list: "Groceries" },

])

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
    todos.value.push({ text: inputText, id: crypto.randomUUID(), done: false, list: currentList.value})
    console.table(todos.value)
}
const removeTodo = (todo) => {
    todos.value = todos.value.filter((i) => i != todo)
}
</script>



<template>
    <Navigation :lists="lists" :currentList="currentList" @change-list="changeList" @add-new-list="addNewList" />
    <List :currentList="currentList" :todos="todos" @add-todo="addTodo" @remove-todo="removeTodo"/>
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