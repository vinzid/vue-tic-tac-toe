<template>
  <div>
    <div class="board-row" v-for="(row, index) in board" :key="index">
      <Square v-for="square in row" :key="square" :square="square" :value="squares[square]" />
    </div>
  </div>
</template>

<script>
import Square from './Square.vue';

export default {
  name: 'Board',
  data() {
    return {
      board: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8]
      ]
    }
  },
  props: ['squares'],
  components: {
    Square
  }
}
</script>