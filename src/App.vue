<script setup>
import { ref, computed } from 'vue'

// 玩家X
const player = ref('X');
// 九宮格
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', ''],
]
);

const calculateWinner = (squares) => {
  // 勝利座標組合
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];
  // for循環每個lines的組合
  for (let i = 0; i < lines.length; i++) {
    // 再組合中給abc值
    const [a, b, c] = lines[i];
    // 檢查squares裡的abc是否有相同的值且不為空,第一個squares[a]是確保它不為null或undefined
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      // 勝利者
      return squares[a];
    }
  }
  // 假設沒有勝利者
  return null;
}

// 計算勝利者 flat()可將參數傳入board.value進行檢查(calculateWinner之前定義好的勝利組合)
const winner = computed(() => calculateWinner(board.value.flat()))

// 玩家移動行跟列
const makeMove = (x, y) => {
  // 如果已有勝利者,直接返回
  if (winner.value) return;
  // 如果座標已有值,也直接返回
  if (board.value[x][y] !== '') return;
  // 當前玩家的座標放置於board.value[x][y]
  board.value[x][y] = player.value
  // 切換玩家
  player.value = player.value === 'X'? 'O' : 'X'
}

// 重置遊戲
const ResetGame = () => {
  // 九宮格清空
  board.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ]
  // 玩家從X開始
  player.value = 'X'
}
</script>

<template>
    <main class="pt-8 text-center dark:bg-gray-800 min-h-screen dark:text-white">
    <h1 class="mb-8 text-3xl font-bold">井字遊戲</h1>
    <h3 class="text-xl mb-4">玩家 {{ player }} 開始 </h3>
    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div v-for="(cell, y) in row" :key="y" @click="makeMove(x, y)" :class="{ 'border': true, 'border-white': true, 'w-20': true, 'h-20': true, 'hover:bg-gray-700': true, 
        'flex items-center justify-center material-icons-outlined text-4xl cursor-pointer': true, 'text-pink-500': cell === 'X',
       'text-blue-400': cell === 'O'}">
        {{ cell === 'X' ? 'close': cell === 'O' ? 'circle' : '' }}
      </div>
      </div>
    </div>
    <h2 v-if="winner" class="text-6xl font-bold mb-8">玩家 "{{ winner }}" 勝利!</h2>
    <button @click="ResetGame" class="px-4 py-2 bg-pink-500 rounded font-bold hover:bg-pink-600 duration-300">重新開始</button>
  </main>
</template>

<style scoped>

</style>
