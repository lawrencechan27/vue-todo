<script setup>
import { ref } from 'vue'

const props = defineProps({
    currentList: String,
    todos: Array,
})

const inputText = ref(null)

// Editing
const editText = ref(null)
const editing = ref(null)
function editTodo(todo) {
    editText.value = todo.text
    editing.value = todo
}
</script>


<template>
    <div class="wrapper">
        <h1>{{ currentList }}</h1>
        <div class="list">
            <div v-for="todo in todos" :key="todo.id">
                <!-- Only show todo items from currentList -->
                <div v-if="todo.list === props.currentList" class="todo" :class="todo.done ? 'completed' : ''">
                    <div v-if="editing != todo">{{ todo.text }}</div>
                    <div v-else><input type="text" v-model="editText" class="editField" /></div>
                    <div>
                        <!-- Editing -->
                        <button v-if="editing != todo" @click="editTodo(todo)">E</button>
                        <button v-else @click="$emit('saveEdit', todo, editText); editing = false; editText = null"
                            class="saveBtn">S</button>
                        <!-- Removing -->
                        <button @click="$emit('removeTodo', todo)">X</button>
                        <!-- Completing -->
                        <button @click="todo.done = !todo.done">C</button>
                    </div>
                </div>
            </div>
        </div>
        <!-- Adding -->
        <div class="input">
            <input type="text" v-model="inputText" /><button
                @click="$emit('addTodo', inputText); inputText = ''">Add</button>
        </div>
    </div>
</template>


<style scoped>
.wrapper {
    background-color: #f2f2f2;
    padding: 10px 20px 20px 20px;
    border-radius: 20px;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

h1 {
    margin: 0 0 10px 0;
    padding: 0;
    font-size: 1.5rem;
}

.todo {
    width: 100%;
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
    width: 100%;
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

.completed {
    background-color: lightyellow;
    text-decoration: line-through;
}
</style>