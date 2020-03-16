<template>
  <button class="square" @click="setValue()">
    {{ value }}
  </button>
</template>

<script>
export default {
  name: 'Square',
  props: ['square', 'value'],
  inject: ['handleClick'],
  methods: {
    setValue() {
      this.handleClick(this.square);
    }
  }
}
</script>