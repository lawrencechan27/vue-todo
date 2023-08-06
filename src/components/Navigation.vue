<script setup>
import { ref } from 'vue'

// To get rid of random console error message
defineOptions({
    inheritAttrs: false
})

// Define props passed from parent component
const props = defineProps({
    lists: Array,
    currentList: Object,
})

const menuIsOpen = ref(false)

const createModalIsOpen = ref(false)
const newListName = ref(null)

// Function to create a new list
const createNewList = () => {
    menuIsOpen.value = false;
    createModalIsOpen.value = true;
}

// Delete list modal toggle
const deleteModalIsOpen = ref(false)

// Rename list modal toggle
const renameModalIsOpen = ref(false)


</script>



<template>
    <!-- New list popup -->
    <div v-if="createModalIsOpen" class="modal-bg">
        <div class="modal">
            <input v-model="newListName" /><button
                @click="$emit('addNewList', newListName); createModalIsOpen = false; newListName = null">Add
                List</button><button @click="createModalIsOpen = false; newListName = null">Cancel</button>
        </div>
    </div>

    <!-- Rename modal modal -->
    <div v-if="renameModalIsOpen" class="modal-bg">
        <div class="modal">
            <input v-model="renameListInput" />
            <button @click="$emit('renameList', renameListInput);renameModalIsOpen = false">Rename</button>
            <button @click="renameModalIsOpen = false">Cancel</button>
        </div>
    </div>

    <!-- Delete confirmation modal -->
    <div v-if="deleteModalIsOpen" class="modal-bg">
        <div class="modal">
            Are you sure? <button @click="$emit('deleteList'); deleteModalIsOpen = false">Yes</button><button
                @click="deleteModalIsOpen = false">No</button>
        </div>
    </div>

    <div class="wrapper">
        <div>
            <!-- v-if to only load button tag once currentList has loaded as an object -->
            <div v-if="currentList" @click="menuIsOpen = !menuIsOpen" class="open-menu-btn">
                <img src="../assets/menu.svg" class="menu-icon">{{ props.currentList.name }}
            </div>
            <!-- Dropdown Menu to Change Lists -->
            <transition appear name="dropdown">
                <div v-if="menuIsOpen" class="dropdown">
                    <button v-for="list in lists" class="dropdown-button"
                        @click="$emit('changeList', list); menuIsOpen = false">
                        {{ list.name }}
                    </button>
                    <button @click="createNewList" class="dropdown-button"><strong>Create New List</strong></button>
                </div>
            </transition>
        </div>
        <div>
            <button @click="renameModalIsOpen = true; renameListInput = currentList.name">Rename</button>
            <button @click="deleteModalIsOpen = true">Delete</button>
        </div>
    </div>
</template>



<style scoped>
.wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.modal-bg {
    z-index: 1;
    width: 100vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.5);
    position: absolute;
    top: 0;
    left: 0;
    display: flex;
    justify-content: center;
    align-items: center;
}

.modal {
    z-index: 2;
    background: white;
    padding: 20px;
    border-radius: 20px;
}

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
    /* transform: scaleY(0.8); */
    /* transform-origin: top; */
}

.dropdown-button {
    width: 100%;
    font-size: 1rem;
    padding: 5px;
    text-align: left;
}
</style>