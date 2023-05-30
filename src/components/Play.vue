<script setup lang="ts">
import {ref} from 'vue';
import Start from './Start.vue';
import PlayTabel from './PlayTabel.vue';

    const playerX = ref("");
    const playerO = ref("");

const addPlayers = (texts: string[]) => {
    console.log(texts);
    playerO.value = texts[0];
    playerX.value = texts[1];

    localStorage.setItem("Players", JSON.stringify({playerO, playerX}));

};

</script>

<template>
    <Start @addPlayers="addPlayers"></Start>
    <PlayTabel></PlayTabel>
</template>

<style scoped>

</style>