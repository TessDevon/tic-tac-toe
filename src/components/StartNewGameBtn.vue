<script setup lang="ts">

import { ref } from "vue";

import { PlaySquare } from '../Models/GameTabel';


const emits = defineEmits<{ (e: "restartNewGame"): void }>();

const handleNewGameBtn = () => {
    emits ("restartNewGame");
};

</script>

<template>
    <button @click="handleNewGameBtn">Starta nytt spel</button>
</template>

<style scoped>
    button {
        border: 2px solid black;
        border-radius: 10px;
        margin:10px
    }
    button:hover{
        background-color: lightgray;
    }
</style>