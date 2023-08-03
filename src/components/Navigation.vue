<script setup>
import { ref } from 'vue'
// Define props passed from parent component (lists)
defineProps({
    lists: Array
})

const currentList = ref("Tasks");

const changeList = (list) => {
    currentList.value = list
    console.log(currentList.value)
}
</script>

<template>
    <div class="wrapper">
        <button v-for="list in lists" class="button" @click="changeList(list)">
            {{ list }}
        </button>
    </div>
</template>

<style scoped>
.wrapper {
    display: grid;
}

.button {
    width:100%;
}
</style>