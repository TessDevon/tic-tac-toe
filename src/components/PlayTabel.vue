<script setup lang="ts">
import { PlaySquare } from "../Models/IGameTabel";
//import { ShowPlayPlan} from "../components/Play.vue"
//import { props } from 'vue';
//import { GameTabel } from '../Models/IGameTabel';


/*const GameTabelPlan = [{symbol:"", id:0 },{symbol:"", id:1},{symbol:"", id:2},{symbol:"", id:3},{symbol:"", id:4},{symbol:"", id:5},{symbol:"", id:6},{symbol:"", id:7},{symbol:"", id:8}];
console.log(GameTabelPlan);*/

interface ShowPlayPlanProps {
    playSquare:PlaySquare; 
};

const props = defineProps<ShowPlayPlanProps>();

</script>

<template>
    <div class="playPart">
    {{ props.playSquare.symbol }}
    </div>
</template>

<style scoped>

.playPart {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 20px;                        /*Större storlek när jag inte längre vill se all info i blocken.*/ 
    height: 100%;
    width: 100%;
    border: 3px solid black;
    border-radius: 6px;
    cursor: pointer;
}

.playPart:hover{
    background-color: lightgray;
}

</style>