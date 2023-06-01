<script setup lang="ts">
import { onMounted, ref } from "vue";
import Start from "./Start.vue";
import PlayTabel from "./PlayTabel.vue";
import Header from "./Header.vue";
import PointBtn from "./PointBtn.vue";
import StartFromOBtn from "./StartFromOBtn.vue";
import StartNewGameBtn from "./StartNewGameBtn.vue";
import { PlaySquare } from "../Models/IGameTabel";
import { Player } from "../Models/IPlayer";

//Add and handle Players
let playerX = ref(new Player("", "X"));
let playerO = ref(new Player("", "O"));

const addPlayers = (texts: string[]) => {
  //Kolla av om spelarna redan finns i localstorage:
  const activePlayers = localStorage.getItem("Players");
  const checkActivePlayers = activePlayers ? JSON.parse(activePlayers) : null;

  if (checkActivePlayers != null) {
    playerO.value.name = checkActivePlayers.name[0];
    playerX.value.name = checkActivePlayers.name[1];
  } else {
    //  console.log(texts);
    playerO.value.name = texts[0];
    playerX.value.name = texts[1];
    localStorage.setItem(
      "Players",
      JSON.stringify({ playerO: playerO.value, playerX: playerX.value })
    );
  }
};

//Add and handle PlayArea
const playSquares = ref<typeof GameTabelPlan>([]);

const GameTabelPlan = [
  { symbol: "", id: 0 },
  { symbol: "", id: 1 },
  { symbol: "", id: 2 },
  { symbol: "", id: 3 },
  { symbol: "", id: 4 },
  { symbol: "", id: 5 },
  { symbol: "", id: 6 },
  { symbol: "", id: 7 },
  { symbol: "", id: 8 },
];

onMounted(() => {
  //Kallas när sidan laddas om sidan.
  let SavedPlay = JSON.parse(localStorage.getItem("SavedPlay") || "[]");
  if (SavedPlay.length == 0) {
    playSquares.value = GameTabelPlan;
  } else {
    playSquares.value = SavedPlay;
  }
  //Kollar i localStorage om aktiv användare finns.
  let SavedActivePlayer = JSON.parse(
    localStorage.getItem("SavedActivePlayer") || "[]"
  );

  if (SavedActivePlayer !== "") {
    activePlayer.value.ID = SavedActivePlayer;
  }
});

let activePlayer = ref<Player>(playerO.value);

//Ändra värde i rutan.

let gameIsOver = false;

const changeValueInplaySquare = (id: number) => {
  if (gameIsOver) {
    return;
  }

  playSquares.value = playSquares.value.map((playSquare) => {
    if (playSquare.id === id) {
      playSquare.symbol = activePlayer.value.ID;
    }
    return playSquare;
  });

  localStorage.setItem("SavedPlay", JSON.stringify(playSquares.value)); //Spara draget i varje drag.

  checkForWinner();
  let players = JSON.parse(localStorage.getItem("Players") || "[]");
  if (activePlayer.value.ID === players.playerO.ID) {
    activePlayer.value = players.playerX; //Växla spelare
  } else {
    activePlayer.value = players.playerO;
  }
  playInfo.value = `Det är ${activePlayer.value.name} tur!`;

  localStorage.setItem(
    "SavedActivePlayer",
    JSON.stringify(activePlayer.value.ID)
  );
};

const checkForWinner = () => {
  const winningOpportunities = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (let winningOpportunity of winningOpportunities) {
    const [position1, position2, position3] = winningOpportunity;

    if (
      playSquares.value[position1].symbol !== "" &&
      playSquares.value[position1].symbol ===
        playSquares.value[position2].symbol &&
      playSquares.value[position1].symbol ===
        playSquares.value[position3].symbol
    ) {
      gameIsOver = true;
      playInfo.value = `Grattis ${activePlayer.value.name}! Du vann!`;
      return;
    }
  }

  const holePlanFull = GameTabelPlan.every((square) => square.symbol !== "");

  if (holePlanFull) {
    gameIsOver = true;
    playInfo.value = `Ingen vann, oavgjort!`;
    return;
  }

  return;
};

let playInfo = ref("");
</script>

<template>
  <Start @addPlayers="addPlayers"></Start>
  <Header></Header>
  <p>{{ playInfo }}</p>
  <div class="playContainer">
    <PlayTabel
      class="playPart"
      @changeValueInplaySquare="changeValueInplaySquare"
      v-for="playSquare in playSquares"
      :playSquare="playSquare"
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
