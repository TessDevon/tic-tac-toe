<script setup lang="ts">
import { onMounted, ref } from "vue";
import Start from "./Start.vue";
import PlayTabel from "./PlayTabel.vue";
import Header from "./Header.vue";
import PointBtn from "./PointBtn.vue";
import StartFromOBtn from "./StartFromOBtn.vue";
import StartNewGameBtn from "./StartNewGameBtn.vue";
import ShowResult from "./ShowResult.vue";
import { PlaySquare } from "../Models/GameTabel";
import { Player } from "../Models/Player";
import { PlayerResult } from "../Models/GameResult";

let noUser = ref(true);

//Add and handle Players
let playerX = ref(new Player("", "X"));
let playerO = ref(new Player("", "O"));
let ResultPlayerO = ref(new PlayerResult("", 0));
let ResultPlayerX = ref(new PlayerResult("", 0));

const addPlayers = (texts: string[]) => {
  playerO.value.name = texts[0];
  playerX.value.name = texts[1];
  localStorage.setItem(
    "Players",
    JSON.stringify({ playerO: playerO.value, playerX: playerX.value })
  );

  ResultPlayerO.value.name = texts[0];
  ResultPlayerX.value.name = texts[1];
  localStorage.setItem(
    "PlayersResult",
    JSON.stringify([ResultPlayerO.value, ResultPlayerX.value])
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
    checkForWinner();
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
    if (!checkForWinnerCalculation()) {
      playInfo.value = `Ingen vann, oavgjort!`;
    } else {
      playInfo.value = `Grattis ${activePlayer.value.name}! Du vann!`;
      let getPlayerToAddPoint = JSON.parse(
        localStorage.getItem("PlayersResult") || "[]"
      );
      getPlayerToAddPoint = getPlayerToAddPoint.map(
        (AddPointToPlayer: PlayerResult) => {
          if (AddPointToPlayer.name === activePlayer.value.name) {
            AddPointToPlayer.points += 1;
          }
          return AddPointToPlayer;
        }
      );
      localStorage.setItem(
        "PlayersResult",
        JSON.stringify(getPlayerToAddPoint)
      );
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
  checkForWinnerCalculation();
  checkForEqualGame();
};

const checkForWinnerCalculation = () => {
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
      return true;
    }
  }
  return false;
};

const checkForEqualGame = () => {
  const holePlanFull = GameTabelPlan.every((square) => square.symbol !== "");

  if (holePlanFull) {
    gameIsOver = true;
    return true;
  }
  return false;
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
  localStorage.removeItem("PlayersResult");
  localStorage.removeItem("SavedResult");
  location.reload();
};

//Show Result
let name1 = ref("");
let name2 = ref("");
let point1 = ref("");
let point2 = ref("");
let showResultDiv = ref(false);

const showResult = () => {
  console.log("hej");
  let showResultPlayers = JSON.parse(
    localStorage.getItem("PlayersResult") || "[]"
  );
  console.log(showResultPlayers);

  showResultDiv.value = true;

  let nameplayer1 = showResultPlayers[0].name;
  let nameplayer2 = showResultPlayers[1].name;
  let pointplayer1 = showResultPlayers[0].points;
  let pointplayer2 = showResultPlayers[1].points;

  name1.value = `${nameplayer1} har  `;
  name2.value = `${nameplayer2} har  `;
  point1.value = `${pointplayer1} poäng`;
  point2.value = `${pointplayer2} poäng`;
};
const closeResult = () => {
  showResultDiv.value = false;
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
    <PointBtn @showResult="showResult"></PointBtn>
    <StartFromOBtn
      @restartWithNewPlayers="restartWithNewPlayers"
    ></StartFromOBtn>
    <StartNewGameBtn @restartNewGame="restartNewGame"></StartNewGameBtn>
  </template>
  <div class="showResultDiv" v-if="showResultDiv">
    <h2 class="resultText">Poängställning</h2>
    <span class="resultInfo">{{ name1 }}</span>
    <span class="resultInfo">{{ point1 }}</span> <br />
    <br />
    <span class="resultInfo">{{ name2 }}</span>
    <span class="resultInfo">{{ point2 }}</span> <br />
    <br />
    <button class="closeResultBtn" @click="closeResult">X</button>
  </div>
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

.playInfo {
  font-size: 2rem;
  color: blue;
}
.showResultDiv {
  position: relative;
  top: -900px;
  left: -380px;
  width: 100vw;
  height: 130vh;
  background-color: rgb(23, 11, 154);
  color: white;
}

.resultText {
  padding-top: 20rem;
  font-size: xx-large;
}

.resultInfo {
  font-size: xx-large;
  margin-top: 10px;
}

.closeResultBtn {
  border: 2px solid black;
  border-radius: 10px;
  margin: 10px;
}

.closeResultBtn:hover {
  background-color: lightgray;
}
</style>
