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

let noUser = ref(true);

//Add and handle Players
let playerX = ref(new Player("", "X"));
let playerO = ref(new Player("", "O"));

const addPlayers = (texts: string[]) => {
  playerO.value.name = texts[0];
  playerX.value.name = texts[1];
  localStorage.setItem(
    "Players",
    JSON.stringify({ playerO: playerO.value, playerX: playerX.value })
  );
  noUser.value = false;
  playInfo.value = `Det är ${activePlayer.value.name} tur!`;
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
  // Show game tableplan
  let SavedPlay = JSON.parse(localStorage.getItem("SavedPlay") || "[]");
  if (SavedPlay.length == 0) {
    playSquares.value = GameTabelPlan;
  } else {
    playSquares.value = SavedPlay;
  }

  //Search for players in LS.
  const activePlayers = localStorage.getItem("Players");
  const checkActivePlayers = activePlayers ? JSON.parse(activePlayers) : null;

  if (checkActivePlayers != null) {
    playerO.value.name = checkActivePlayers.playerO.name;
    playerX.value.name = checkActivePlayers.playerX.name;
    noUser.value = false;
    playInfo.value = `Det är ${activePlayer.value.name} tur!`;
  }

  //Search for active plyer in LS
  let SavedActivePlayer = JSON.parse(
    localStorage.getItem("SavedActivePlayer") || "O"
  );
  if (SavedActivePlayer !== "") {
    activePlayer.value.ID = SavedActivePlayer;
  }
});

let activePlayer = ref<Player>(playerO.value);

//Change the value in the squares
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

  if (gameIsOver) {
    const holePlanFull = GameTabelPlan.every((square) => square.symbol !== "");
    if (holePlanFull) {
      playInfo.value = `Ingen vann, oavgjort!`;
    } else {
      playInfo.value = `Grattis ${activePlayer.value.name}! Du vann!`;
    }
    return;
  }

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

//Start new game
const restartNewGame = () => {
  localStorage.removeItem("SavedPlay");
  localStorage.removeItem("SavedActivePlayer");
  location.reload();
};

// Restart game from beginnig
const restartWithNewPlayers = () => {
  localStorage.removeItem("SavedPlay");
  localStorage.removeItem("Players");
  localStorage.removeItem("SavedActivePlayer");
  //localStorage.removeItem("SavedResult"); Även resultatet måste tas bort från LS om ngn användare har samma namn så dennes poäng inte kommer med i nästa nya spel...??
  location.reload();
};
</script>

<template>
  <Header></Header>
  <Start @addPlayers="addPlayers" v-if="noUser"></Start>
  <template v-else>
    <p class="playInfo">{{ playInfo }}</p>
    <div class="playContainer">
      <PlayTabel
        class="playPart"
        @changeValueInplaySquare="changeValueInplaySquare"
        v-for="playSquare in playSquares"
        :playSquare="playSquare"
      />
    </div>
    <PointBtn></PointBtn>
    <StartFromOBtn
      @restartWithNewPlayers="restartWithNewPlayers"
    ></StartFromOBtn>
    <StartNewGameBtn @restartNewGame="restartNewGame"></StartNewGameBtn>
  </template>
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

.playInfo{
  font-size: 2rem;
  color: blue;
}
</style>
