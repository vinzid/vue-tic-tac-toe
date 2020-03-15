<template>
  <div id="app">
    <div class="game">
      <div class="game-board">
        <Board :squares="history[history.length - 1].squares" @click="handleClick" />
      </div>
      <div class="game-info">
        <div>{{ status }}</div>
        <ol>{{ /* TODO */ }}</ol>
      </div>
    </div>
  </div>
</template>

<script>
import Board from './components/Board.vue'

export default {
  name: 'App',
  data() {
    return {
      history: [{
        squares: Array(9).fill(null),
      }],
      xIsNext: true,
      status: 'Next player: X'
    }
  },
  methods: {
    handleClick(i) {
      const history = this.history;
      const current = history[history.length - 1]
      const squares = current.squares.slice();
      if (calculateWinner(squares)) {
        alert('Winner was determined!');
        return;
      }
      if (squares[i]){
        alert('Place was taken!');
        return
      }
      squares[i] = this.xIsNext ? 'X' : 'O';
      history.push({
        squares: squares
      });
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
    Board
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

<style>
body {
  font: 14px "Century Gothic", Futura, sans-serif;
  margin: 20px;
}

ol, ul {
  padding-left: 30px;
}

.board-row:after {
  clear: both;
  content: "";
  display: table;
}

.status {
  margin-bottom: 10px;
}

.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.square:focus {
  outline: none;
}

.kbd-navigation .square:focus {
  background: #ddd;
}

.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}
</style>