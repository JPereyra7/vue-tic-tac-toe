<script setup lang="ts">
import { ref, defineProps, onMounted } from 'vue';

interface Player {
  name: string;
  symbol: 'X' | 'O';
}

const props = defineProps<{ players: Player[] }>();
const currentPlayer = ref<'X' | 'O'>('X');
const winner = ref<Player | null>(null);
const isDraw = ref<boolean>(false);

const gameBoard = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
]);

const winningPlayer = (squares: string[]): Player | null => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return props.players.find(player => player.symbol === squares[a]) || null;
    }
  }
  return null;
}

const gameStatusUpdate = () => {
  const flatBoard = gameBoard.value.flat();
  winner.value = winningPlayer(flatBoard);
  isDraw.value = !winner.value && flatBoard.every(cell => cell !== '');
};

const MakingAMove = (x: number, y: number) => {
  if (winner.value || isDraw.value) return;
  if (gameBoard.value[x][y] !== '') return;
  gameBoard.value[x][y] = currentPlayer.value;
  gameStatusUpdate();
  if (!winner.value) {
    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
  }
};

const gameResetting = () => {
  gameBoard.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ];
  currentPlayer.value = 'X';
  winner.value = null;
  isDraw.value = false;
};

onMounted(() => {
  gameBoard;
  winningPlayer;
  gameResetting;
});
</script>

<template>
  <div class="mainContainer">
    <div class="fontContainer">
      <h1 class="headerTitle">Tic Tac Toe</h1>
      <h2 v-if="winner" class="winningText">Player '{{ winner.name }} ({{ winner.symbol }})' wins!</h2>
      <h2 v-else-if="isDraw">Draw!</h2>
      <h3 v-else>Player {{ currentPlayer === 'X' ? props.players[0].name : props.players[1].name }}'s turn</h3>
    </div>

    <div class="gridContainer">
      <div 
        v-for="(row, x) in gameBoard"
        :key="x"
        class="flexContainer">

        <div 
          v-for="(cell, y) in row"
          :key="y"
          @click="MakingAMove(x, y)"
          class="flexChild"
          :data-cell="cell">
          {{ cell }}
        </div>
      </div>
    </div>
    <button class="zeButton" @click="gameResetting">Retry</button>
  </div>

</template>

<style scoped>
.fontContainer {
  display: flex;
  flex-direction: column;
  gap: 2em;
  margin-bottom: 2em;
  letter-spacing: 2px;
}

.mainContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  text-align: center;
}

.gridContainer {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  border: 1px solid black;
  max-width: 15rem;
}

.flexContainer {
  display: contents;
}

.flexChild {
  display: flex;
  width: 4.8rem;
  height: 5rem;
  font-size: 2.25rem;
  cursor: pointer;
  border: 1px solid rgb(160, 179, 185);
  align-items: center;
  justify-content: center;
  color: white;
  text-shadow: 
    0 0 7px #ffffff,
    0 0 1px #fff,
    0 0 21px #fff,
    0 0 42px rgb(0, 242, 255),
    0 0 52px rgb(0, 221, 255);

  &[data-cell='O'] {
    color: white;
  text-shadow: 
    0 0 7px #f7e7e7,
    0 0 1px #fff,
    0 0 21px #f6bcbc,
    0 0 42px rgb(255, 55, 0),
    0 0 52px rgb(252, 129, 127);
  }

  &:hover {
    background-color: rgba(0, 0, 0, 0.438);
  }
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

.winningText {
  text-align: center;
  color: white;
  letter-spacing: 2px;
}

.zeButton {
        margin-top: 2em;
        box-shadow: 
        0 0 7px #ffffff,
        0 0 1px #fff,
        0 0 42px rgb(0, 242, 255);
        margin-bottom: 2em;
        border: none;

        &:hover {
        transition: 0.8s;
        box-shadow: 
        0 0 7px #ffffff,
        0 0 1px #ffeacb,
        0 0 42px rgb(255, 166, 0);
        border: none;
        }
        &:active {
            outline: none;
            border: none;
        }
    }

</style>
