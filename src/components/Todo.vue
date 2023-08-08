<script setup>
import { ref, onMounted, watch, watchEffect } from 'vue';
import Navigation from './Navigation.vue'
import List from './List.vue'

// Define vars
const lists = ref([])
const currentList = ref(null)
const todos = ref([])

// Check if todos exist in local storage, and get them
function fetchTodos() {
    const savedLists = JSON.parse(localStorage.getItem("lists"))
    const savedCurrentList = JSON.parse(localStorage.getItem("currentList"))
    const savedTodos = JSON.parse(localStorage.getItem("todos"))
    // If savedCurrentList exists, fill variables with JSON data
    if (savedCurrentList) {
        lists.value = savedLists
        currentList.value = savedCurrentList
        todos.value = savedTodos
    } else {
        // Otherwise add default list
        lists.value = [{ name: "Tasks", id: crypto.randomUUID() }]
        currentList.value = lists.value[0]
    }
}

onMounted(() => {
    fetchTodos();
})


// Watch for any changes to lists, currentList or todos, then run save function if change is detected
watch([lists, currentList, todos], () => {
    saveToLocalStorage()
}, { deep: true }) // deep: true allows watcher to reach props

function saveToLocalStorage() {
    localStorage.setItem("lists", JSON.stringify(lists.value))
    localStorage.setItem("currentList", JSON.stringify(currentList.value))
    localStorage.setItem("todos", JSON.stringify(todos.value))
}

// watchEffect, alternativet o watch, but not working
// watchEffect(async () => {
//     localStorage.setItem("lists", JSON.stringify(lists.value))
//     localStorage.setItem("currentList", JSON.stringify(currentList.value))
//     localStorage.setItem("todos", JSON.stringify(todos.value))
// })

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
    // Check if list name exists
    if (lists.value.some(e => e.name === newListName)) {
        alert("List with that name already exists")
    } else {
        lists.value.push({ name: newListName, id: crypto.randomUUID() })
        // Change current list to new list
        currentList.value = lists.value[lists.value.length - 1]
    }
}
const renameList = (renameListInput) => {
    // Check if list name exists
    if (lists.value.some(e => e.name === renameListInput)) {
        alert("List with that name already exists")
    } else {
        currentList.value.name = renameListInput
    }
}
const deleteList = () => {
    // Check to see if it's the only list, show error if it is
    if (lists.value.length > 1) {
        lists.value = lists.value.filter((i) => i != currentList.value)
        // Delete todos associated with the list also
        todos.value = todos.value.filter(
            (t) => t.list != currentList.value.id
        )
        // Set current list to whatever is first in array.. AFTER removing todos
        currentList.value = lists.value[0]
    } else {
        alert("Can't delete the only list")
    }
}
// Functions emitted from List component
const addTodo = (inputText) => {
    todos.value.push({ text: inputText, id: crypto.randomUUID(), done: false, list: currentList.value.id })
}
const removeTodo = (todo) => {
    todos.value = todos.value.filter((i) => i != todo)
}
const saveEdit = (todo, editText) => {
    todo.text = editText
}

</script>



<template>
    <Navigation :lists="lists" :currentList="currentList" @change-list="changeList" @add-new-list="addNewList"
        @rename-list="renameList" @delete-list="deleteList" />
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