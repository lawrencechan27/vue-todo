<script setup>
import { ref } from 'vue';
import Navigation from './Navigation.vue'
import List from './List.vue'

const lists = ref(["Tasks", "Groceries"])

const todos = ref([
    { text: "Tasks 1", id: "crypto.randomUUID()", done: false, list: "Tasks" },
    { text: "Tasks 2", id: "crypto.randomUUID()", done: false, list: "Tasks" },
    { text: "Tasks 3", id: "crypto.randomUUID()", done: false, list: "Tasks" },
    { text: "Groceries 1", id: "crypto.randomUUID()", done: false, list: "Groceries" },
    { text: "Groceries 2", id: "crypto.randomUUID()", done: false, list: "Groceries" },
    { text: "Groceries 3", id: "crypto.randomUUID()", done: false, list: "Groceries" },
])

</script>

<template>
    <Navigation :lists="lists" />
    <div v-for="list in lists">
        <List :list="list" />
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
</style>