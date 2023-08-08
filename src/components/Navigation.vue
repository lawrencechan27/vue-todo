<script setup>
import { ref } from 'vue'
import Modal from './Modal.vue'

// To get rid of random console error message
defineOptions({
    inheritAttrs: false
})

// Define props passed from parent component
const props = defineProps({
    lists: Array,
    currentList: Object,
})

const changeModalActive = ref(false)

const createModalActive = ref(false)
const newListName = ref(null)

// Function to create a new list
const createNewList = () => {
    changeModalActive.value = false;
    createModalActive.value = true;
}

// Delete list modal toggle
const deleteModalActive = ref(false)

// Rename list modal toggle
const renameModalActive = ref(false)
</script>



<template>
    <!-- Change list modal -->
    <Modal v-model="changeModalActive">
        <div class="change-modal">
            <button v-for="list in lists" @click="$emit('changeList', list); changeModalActive = false">
                {{ list.name }}
            </button>
            <button @click="createNewList" class="dropdown-button"><strong>Create New List</strong></button>
        </div>
    </Modal>

    <!-- Create list modal -->
    <Modal v-model="createModalActive">
        <input v-model="newListName" /><button
            @click="$emit('addNewList', newListName); createModalActive = false; newListName = null">Add
            List</button>
    </Modal>

    <!-- Rename modal -->
    <Modal v-model="renameModalActive">
        <input v-model="renameListInput" />
        <button @click="$emit('renameList', renameListInput); renameModalActive = false">Rename</button>
    </Modal>

    <!-- Delete confirmation modal -->
    <Modal v-model="deleteModalActive">
        <p>Delete {{ currentList.name }}?</p>
        <button @click="$emit('deleteList'); deleteModalActive = false">Confirm</button>
    </Modal>

    <div class="wrapper">
        <div>
            <!-- v-if to only load button tag once currentList has loaded as an object -->
            <div v-if="currentList" @click="changeModalActive = !changeModalActive" class="open-menu-btn">
                <img src="../assets/menu.svg" class="menu-icon">{{ props.currentList.name }}
            </div>
        </div>
        <div>
            <button @click="renameModalActive = true; renameListInput = currentList.name" class="top-buttons">Rename</button>
            <button @click="deleteModalActive = true" class="top-buttons">Delete</button>
        </div>
    </div>
</template>



<style scoped>
.wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 20px;
    margin-bottom:10px;
}

.open-menu-btn {
    display: flex;
    align-items: center;
    cursor: pointer;
    color: white;
    font-size: 1.5rem;
    gap: 10px;
}

button {
    background:lightblue;
    color: white;
    border: none;
    padding: 10px;
    font-size: 1rem;
    border-radius: 20px;
}

.menu-icon {
    filter: invert(100%) sepia(0%) saturate(7479%) hue-rotate(70deg) brightness(99%) contrast(107%);
    width: 1.5rem;
    height: 1.5rem;
    color: white;
}

.change-modal {
    display: grid;
    gap:10px;
}
</style>