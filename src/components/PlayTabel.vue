<script setup lang="ts">
import { PlaySquare } from "../Models/GameTabel";

interface ShowPlayPlanProps {
  playSquare: PlaySquare;
}

const props = defineProps<ShowPlayPlanProps>();

defineEmits<{ (e: "changeValueInplaySquare", id: number): void }>();
</script>

<template>
  <div
    @click.once="() => $emit('changeValueInplaySquare', props.playSquare.id)"
    class="playPart"
  >
    {{ props.playSquare.symbol }}
  </div>
</template>

<style scoped>
.playPart {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 150px;
  height: 100%;
  width: 100%;
  border: 3px solid black;
  border-radius: 6px;
  cursor: pointer;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
  font-weight: 600;
}

.playPart:hover {
  background-color: lightgray;
}
</style>
