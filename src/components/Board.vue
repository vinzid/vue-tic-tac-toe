<template>
  <div>
    <div class="status">{{ status }}</div>
    <div class="board-row">
      <Square />
      <Square />
      <Square />
    </div>
    <div class="board-row">
      <Square />
      <Square />
      <Square />
    </div>
    <div class="board-row">
      <Square />
      <Square />
      <Square />
    </div>
  </div>
</template>

<script>
import Square from './Square.vue';

export default {
  name: 'Board',
  data() {
    return {
      status: 'Next player: X'
    }
  },
  components: {
    Square
  }
}
</script>