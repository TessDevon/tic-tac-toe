<script setup lang="ts">
import { onMounted, ref } from 'vue';
import Start from './Start.vue';
import PlayTabel from './PlayTabel.vue';
import Header from './Header.vue';
import PlayerInfo from './PlayerInfo.vue';
import PointBtn from './PointBtn.vue';
import StartFromOBtn from './StartFromOBtn.vue';
import StartNewGameBtn from './StartNewGameBtn.vue';
//import { PlaySquare } from '../Models/IGameTabel';

//Handle Players
const playerX = ref("");
const playerO = ref("");

const addPlayers = (texts: string[]) => {
    console.log(texts);
    playerO.value = texts[0];
    playerX.value = texts[1];

    localStorage.setItem("Players", JSON.stringify({playerO, playerX}));
};

//Add and handle Playarea
const playSquares = ref<typeof GameTabelPlan>([])

const GameTabelPlan = [{symbol:"r", id:0 },{symbol:"", id:1},{symbol:"", id:2},{symbol:"", id:3},{symbol:"", id:4},{symbol:"", id:5},{symbol:"", id:6},{symbol:"", id:7},{symbol:"", id:8}];

onMounted(() => {
    let SavedPlay =  JSON.parse(localStorage.getItem("SavedPlay") || "[]") ;
    if (SavedPlay.length == 0){
        playSquares.value = GameTabelPlan; 
    } else {
        playSquares.value = SavedPlay;
    }            
    //console.log(playSquares.value);
});


</script>

<template>
    <Start @addPlayers="addPlayers"></Start>
    <Header></Header>
    <PlayerInfo></PlayerInfo>
    <div class="playContainer">
        <PlayTabel class="playPart" v-for="playSquare in playSquares" :playSquare="playSquare" />
    </div>
    <PointBtn></PointBtn>
    <StartFromOBtn></StartFromOBtn>
    <StartNewGameBtn></StartNewGameBtn>

</template>

<style scoped>

.playContainer {
    width: 600px;
    height: 600px;
    display: grid;
    grid-template-columns: 200px 200px 200px;
    grid-template-rows: 200px 200px 200px;
    justify-items: center;
    align-items: center;
    border-radius: 7px;
    /*
    flex-direction: row;
    margin: 0px;
    padding: 0px;
    */
}


</style>