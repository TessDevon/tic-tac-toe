<script setup lang="ts">
import { onMounted, ref } from 'vue';
import Start from './Start.vue';
import PlayTabel from './PlayTabel.vue';
import Header from './Header.vue';
import PointBtn from './PointBtn.vue';
import StartFromOBtn from './StartFromOBtn.vue';
import StartNewGameBtn from './StartNewGameBtn.vue';
import { PlaySquare } from '../Models/IGameTabel';
import { Player } from '../Models/IPlayer';


const playerX = ref(new Player ("", "X"));
const playerO = ref(new Player ("", "O"));


const addPlayers = (texts: string[]) => {
    console.log(texts);
    playerO.value.name = texts[0];
    playerX.value.name = texts[1];

    localStorage.setItem("Players", JSON.stringify({playerO:playerO.value, playerX:playerX.value}));
};

//Add and handle PlayArea
const playSquares = ref<typeof GameTabelPlan>([])

const GameTabelPlan = [{symbol:"", id:0 },{symbol:"", id:1},{symbol:"", id:2},{symbol:"", id:3},{symbol:"", id:4},{symbol:"", id:5},{symbol:"", id:6},{symbol:"", id:7},{symbol:"", id:8}];

onMounted(() => {
    let SavedPlay =  JSON.parse(localStorage.getItem("SavedPlay") || "[]") ;
    if (SavedPlay.length == 0){
        playSquares.value = GameTabelPlan; 
    } else {
        playSquares.value = SavedPlay;
    }            
});
let activePlayer = ref<Player>(playerO.value);

const changeValueInplaySquare = (id:number) => { 
    playSquares.value = playSquares.value.map((playSquare) => {
        
        if (playSquare.id === id) {
            playSquare.symbol = activePlayer.value.ID;
        }
        return playSquare;
    })
    let players = JSON.parse(localStorage.getItem("Players") || "[]");
        if (activePlayer.value.ID === players.playerO.ID) {
            activePlayer.value = players.playerX;
        } else {
            activePlayer.value = players.playerO;
        }
}

</script>

<template>
    <Start @addPlayers="addPlayers"></Start>
    <Header></Header>
    <p>Det är {{ activePlayer.name }} tur!</p>
    <div class="playContainer">
        <PlayTabel 
        class="playPart" 
        @changeValueInplaySquare="changeValueInplaySquare"
        v-for="playSquare in playSquares" :playSquare="playSquare" 
        />
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
}


</style>