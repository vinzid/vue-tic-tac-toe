<template>
  <div>
    <div class="status">Next player: {{ next }}</div>
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
      next: 'X',
      board: [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9]
      ],
      squares: Array(9).fill(null),
      xIsNext: true
    }
  },
  methods: {
    handleClick(i) {
      const squares = this.squares.slice();
      squares[i] = this.xIsNext ? 'X' : 'O';
      this.squares = squares;
      this.xIsNext = !this.xIsNext;
      this.next = this.xIsNext ? 'X' : 'O';
    }
  },
  components: {
    Square
  }
}
</script>