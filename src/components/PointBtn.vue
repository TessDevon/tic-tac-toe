<script setup lang="ts">
const emits = defineEmits<{ (e: "showResult"): void }>();

const handleShowResult = () => {
  emits("showResult");
};
</script>

<template>
  <button @click="handleShowResult">Visa poäng</button>
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
