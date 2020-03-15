<template>
  <div>
    <div class="status">{{ status }}</div>
    <div class="board-row" v-for="(row, index) in board" :key="index">
      <Square v-for="square in row" :key="square" :square="square" :value="squares[square]" @click="handleClick" />
    </div>
  </div>
</template>

<script>
import Square from './Square.vue';

export default {
  name: 'Board',
  data() {
    return {
      status: 'Next player: X',
      board: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8]
      ],
      squares: Array(9).fill(null),
      xIsNext: true
    }
  },
  methods: {
    handleClick(i) {
      const squares = this.squares.slice();
      if (calculateWinner(squares)) {
        alert('Winner was determined!');
        return;
      }
      if (squares[i]){
        alert('Place was taken!');
        return
      }
      squares[i] = this.xIsNext ? 'X' : 'O';
      this.squares = squares;
      const winner = calculateWinner(squares);
      if (winner) {
        this.status = 'Winner: ' + winner;
        return;
      }
      this.xIsNext = !this.xIsNext;
      this.status = `Next player: ${this.xIsNext ? 'X' : 'O'}`;
    }
  },
  components: {
    Square
  }
}

function calculateWinner(squares) {
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
      return squares[a];
    }
  }
  return null;
}
</script>