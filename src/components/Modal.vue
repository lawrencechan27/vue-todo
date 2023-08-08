<script setup>
defineProps({
    modelValue: {
        type: Boolean,
        // required property to log error when v-model isn't defined on <Modal>
        required: true
    }
})
defineEmits([
    'update:modelValue'
])
</script>

<template>
    <transition name="background">
        <div v-if="modelValue" class="background" @click="$emit('update:modelValue', false)">
            <transition name="modal">
                <!-- @click.stop to prevent the click going through Modal and triggering background click -->
                <div v-if="modelValue" class="modal" @click.stop>
                    <button class="close-button" @click="$emit('update:modelValue', false)">✖</button>
                    <slot />
                </div>
            </transition>
        </div>
    </transition>
</template>

<style scoped>
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
    align-items: start;
}

.modal {
    z-index: 2;
    background: white;
    padding: 40px;
    border-radius: 20px;
    position: relative;
    text-align:center;
    margin-top:20px;
}

.close-button {
    z-index: 3;
    position: absolute;
    top: 0;
    right: 0;
    width:40px;
    height:40px;
    border-radius:20px;
    background:white;
    color:lightgrey;
    border:none;
    cursor:pointer;
    font-size:1.5rem;
}

.background-enter-active,
.background-leave-active {
    transition: all 0.3s ease;
}

.background-enter-from,
.background-leave-to {
    opacity: 0;
}

.modal-enter-active,
.modal-leave-active {
    transition: all 0.5s cubic-bezier(.47, 1.64, .41, .8);
}

.modal-enter-from,
.modal-leave-to {
    opacity: 0;
    transform: scale(0.5)
}
</style>