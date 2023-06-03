<script setup lang="ts">
import { ref } from "vue";

const userInputX = ref("");
const userInputO = ref("");

const emits = defineEmits<{ (e: "addPlayers", texts: string[]): void }>();

const handleSubmit = () => {
  emits("addPlayers", [userInputO.value, userInputX.value]);
  userInputO.value = "";
  userInputX.value = "";
};
</script>

<template>
  <h2>Registrera spelare</h2>
  <div>
    <form @submit.prevent="handleSubmit">
      <input
        required
        placeholder="Player O"
        type="text"
        v-model="userInputO"
        id="O"
      />
      <input
        required
        placeholder="Player X"
        type="text"
        v-model="userInputX"
        id="X"
      />
      <button>Spela</button>
    </form>
  </div>
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
input {
  font-size: 1.3rem;
}
</style>
