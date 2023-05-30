<script setup lang="ts">
//import { GameTabel } from '../Models/IGameTabel';


const GameTabelPlan = [{symbol:"", id:0 },{symbol:"", id:1},{symbol:"", id:2},{symbol:"", id:3},{symbol:"", id:4},{symbol:"", id:5},{symbol:"", id:6},{symbol:"", id:7},{symbol:"", id:8}];
console.log(GameTabelPlan);
//GameTabelPlan = GameTabelPlan.values.map((GameTabelPlan) => {
//});


</script>

<template>
    <div>
    <h1>Tre i rad</h1>
    <p></p><!--Hämta upp andändaren och meddela att det är dennes tur-->    
        <div>
            <ul class="playContainer">
                <li class="playPart" v-for="(symbol, value) in GameTabelPlan">
                {{ symbol }}</li>
            </ul>
        </div>
    </div>
</template>

<style scoped>

.playContainer {
    width: 450px;
    height: 450px;
    display: grid;
    grid-template-columns: 150px 150px 150px;
    grid-template-rows: 150px 150px 150px;
    justify-items: center;
    align-items: center;
    /*
    flex-direction: row;
    margin: 0px;
    padding: 0px;
    */
}

.playPart {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 20px;
    height: 100%;
    width: 100%;
    border: 1px solid black;
}

.playPart:hover{
    background-color: lightgray;
}

</style>