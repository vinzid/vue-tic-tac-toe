<template>
  <div>
    <div class="status">{{ status }}</div>
    <div class="board-row" v-for="row in board">
      <Square v-for="square in row" :square="square" :value="squares[square]" @click="handleClick" />
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
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9]
      ],
      squares: Array(9).fill(null),
    }
  },
  methods: {
    handleClick(i) {
      const squares = this.squares.slice();
      squares[i] = 'X';
      this.squares = squares;
    }
  },
  components: {
    Square
  }
}
</script>