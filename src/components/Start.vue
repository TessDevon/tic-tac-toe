<script setup lang="ts">
import { ref } from "vue";

const userInputX = ref("");
const userInputO = ref("");

const emits = defineEmits<{ (e: "addPlayers", texts: string[]): void }>();

const handleSubmit = () => {
    //console.log("handleSubmit");
  emits("addPlayers", [userInputO.value, userInputX.value]);
  userInputO.value = "";
  userInputX.value = "";
};
</script>

<template>
  <h2>Registrera spelare</h2>
  <div>
    <form @submit.prevent="handleSubmit">
      <input placeholder="Player O" type="text" v-model="userInputO" id="O" />
      <input placeholder="Player X" type="text" v-model="userInputX" id="X" />
      <button>Spela</button>
    </form>
  </div>
  
</template>

<style scoped></style>
