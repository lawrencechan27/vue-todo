<script setup>
import { ref } from 'vue'
// Define props passed from parent component
const props = defineProps({
    lists: Array,
    currentList: String,
})

const menuIsOpen = ref(false)

const createDialogueIsOpen = ref(false)
const newListName = ref(null)

// Function to create a new list
const createNewList = () => {
    menuIsOpen.value = false;
    createDialogueIsOpen.value = true;
}
</script>



<template>
    <div v-if="createDialogueIsOpen" class="create-dialogue">
        <input v-model="newListName" /><button @click="$emit('addNewList',newListName); createDialogueIsOpen = false">Add List</button><button @click="createDialogueIsOpen=false">Cancel</button>
    </div>

    <div @click="menuIsOpen = !menuIsOpen" class="open-menu-btn">
        <img src="../assets/menu.svg" class="menu-icon">{{ currentList }}
    </div>
    <transition appear name="dropdown">
        <div v-if="menuIsOpen" class="dropdown">
            <button v-for="list in lists" class="dropdown-button" @click="$emit('changeList', list); menuIsOpen = false">
                {{ list }}
            </button>
            <button @click="createNewList" class="dropdown-button"><strong>Create New List</strong></button>
        </div>
    </transition>
</template>



<style scoped>
.open-menu-btn {
    display: flex;
    align-items: center;
    cursor: pointer;
    color: white;
    font-size: 1.5rem;
    gap: 10px;
}

.menu-icon {
    filter: invert(100%) sepia(0%) saturate(7479%) hue-rotate(70deg) brightness(99%) contrast(107%);
    width: 1.5rem;
    height: 1.5rem;
    color: white;
}

.dropdown {
    z-index: 1;
    position: absolute;
}

.dropdown-enter-active,
.dropdown-leave-active {
    transition: all 0.5s ease;
}

.dropdown-enter-from,
.dropdown-leave-to {
    opacity: 0;
    transform: translateY(-20px)
}

.dropdown-button {
    width: 100%;
    font-size: 1rem;
    padding: 5px;
    text-align: left;
}
</style>