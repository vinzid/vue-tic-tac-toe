<template>
  <div>
    <div class="status">{{ status }}</div>
    <div class="board-row">
      <Square value="1" />
      <Square value="2" />
      <Square value="3" />
    </div>
    <div class="board-row">
      <Square value="4" />
      <Square value="5" />
      <Square value="6" />
    </div>
    <div class="board-row">
      <Square value="7" />
      <Square value="8" />
      <Square value="9" />
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