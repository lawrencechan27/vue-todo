<script setup>
const props = defineProps({
    modalActive: Boolean
})
</script>

<template>
    <transition name="background">
        <div v-show="modalActive" class="background">
            <transition name="modal">
            <div v-if="modalActive" class="modal">
                <button class="close-button" @click="$emit('closeModal')">X</button>
                <slot />
            </div>
            </transition>
        </div>
    </transition>
</template>

<style>
.background {
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
    position: relative;
}

.close-button {
    z-index: 3;
    position: absolute;
    top: 0;
    right: 0;
}

.background-enter-active,
.background-leave-active {
    transition: all 0.5s ease;
}
.background-enter-from,
.background-leave-to {
    opacity: 0;
}

.modal-enter-active,
.modal-leave-active {
    transition: all 0.5s cubic-bezier(.47,1.64,.41,.8);
}
.modal-enter-from,
.modal-leave-to {
    opacity: 0;
    transform: scale(0.5)
}
</style>