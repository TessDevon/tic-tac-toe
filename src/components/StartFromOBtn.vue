<script setup lang="ts">
const emits = defineEmits<{ (e: "restartWithNewPlayers"): void }>();

const handleStartGameFromStart = () => {
  emits("restartWithNewPlayers");
};
</script>

<template>
  <button @click="handleStartGameFromStart">Starta från början</button>
</template>

<style scoped>
button {
  border: 2px solid black;
  border-radius: 10px;
  margin: 10px;
}
button:hover {
  background-color: lightgray;
}
</style>
