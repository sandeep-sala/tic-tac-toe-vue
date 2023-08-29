<script setup>
import { ref } from 'vue';

const player = ref('X');
const winner = ref(null);
const board = ref(Array(9).fill(null));
const win_list = [
  [0, 1, 2],[3, 4, 5],[6, 7, 8],
  [0, 3, 6],[1, 4, 7],[2, 5, 8],
  [0, 4, 8],[2, 4, 6],
]

function check_winner() {
  for (let i = 0; i < win_list.length; i++) {
    const [a, b, c] = win_list[i];
    if(board.value[a] === player.value &&
      board.value[b] === player.value &&
      board.value[c] === player.value){
        return player.value;
      }
  }
  return;
}

function nextMove(index) {
  if(winner.value){
    return;
  }

  if(! board.value[index]){
    board.value[index] = player.value;
    if(check_winner()){
      winner.value = player.value;
    }
    player.value = player.value === 'X' ? 'O' : 'X';
  }
}

function reset_game() {
  winner.value = null;
  board.value = Array(9).fill(null);
}

</script>

<template>
  <div class="main-container">
    <h1>Tic Tac Toe</h1>
    <p>Player: {{ player }}</p>

    <div v-if="winner">
      <p>Winner: {{ winner }}</p>
    </div>

    <div class="grid-container">
      <div class="grid-item"
        v-for="val,index in board" :key="val"
        @click="nextMove(index)"
        >
        <div :class=" val == 'X'? 'x-lass': 'o-lass' ">{{ val }}</div>
      </div>
    </div>

    <button @click="reset_game">Reset</button>

  </div>
</template>

<style scoped>
.main-container{
  width: 98%;
  margin: 0 auto;
  text-align: center;
}
.grid-container {
  display: grid;
  grid-template-columns: auto auto auto;
  background-color: #2196F3;
  width: 24em;
  height: 24em;
  margin: 0 auto;
  grid-gap: 2px;
  min-height: 0;
  min-width: 0;
  box-sizing: border-box;
}
.grid-item {
  background-color: rgba(255, 255, 255, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  box-sizing: border-box;
  width: 100%;
  height: 100%;
  position: relative;
  cursor: pointer;
}
.grid-item>div{
  position: absolute;
  margin-left: auto;
  margin-right: auto;
  left: 0;
  right: 0;
  text-align: center;
  font-size: 8em;
}
.x-lass{
  color: #ff00008c;
}
.o-lass{
  color: #00000094;
}
button{
  background: #ff8300d4;
  border: 0px;
  padding: 10px 34px;
  color: white;
  font-weight: bold;
  margin: 1em;
  cursor: pointer;
}
</style>
