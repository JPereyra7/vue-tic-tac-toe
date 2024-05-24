<script setup lang="ts">
import AddPlayer from "../components/AddPlayer.vue";
import { ref } from "vue";
import Gameboard from "../components/Gameboard.vue";

interface Player {
  name: string;
  symbol: 'X' | 'O';
}

const playerList = ref<Player[]>([]);
const hideGame = ref(false);
const hidePlayers = ref("");
const hideTitle = ref("");

const addPlayer = (playerName: string) => {
  if (playerList.value.length < 2) {
    const symbol: 'X' | 'O' = playerList.value.length === 0 ? 'X' : 'O';
    playerList.value.push({name: playerName, symbol: symbol});
    console.log(playerList.value);
  } else {
    alert("Maximal antal spelare");
  }
};

const startGame = (btn: boolean) => {
  hideGame.value = btn;
  hidePlayers.value = "hidePlayers";
  hideTitle.value = "hidePlayers";
};
</script>

<template>
  <div>
    <div :class="hidePlayers">
    <h1 class="headerTitle">Tic Tac Toe</h1>
    </div>
    <AddPlayer @add-player="addPlayer" @start-btn="startGame" />
    <div :class="hidePlayers">
      <h2 class="shownPlayers">Spelare:</h2>
      <h4 class="shownPlayers" v-for="(player, index) in playerList" :key="index">{{ player.name }} ({{ player.symbol }})</h4>
    </div>

    <div>
      <Gameboard v-if="hideGame" :players="playerList" />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.hideGameClass {
  display: none;
}

.hidePlayers {
  display: none;
}

.headerTitle {
  letter-spacing: 2px;
  color: white;
  text-shadow: 
      0 0 7px #ffffff,
      0 0 1px #fff,
      0 0 21px #fff,
      0 0 42px rgb(0, 242, 255),
      0 0 52px rgb(0, 221, 255);
}

.shownPlayers {
  font-family: Poppins, sans-serif;
    font-weight: 600;
    letter-spacing: 2px;
    color: white;
    text-shadow: 
        0 0 7px #ffffff,
        0 0 1px #fff,
        0 0 42px rgb(0, 242, 255),
}

</style>
