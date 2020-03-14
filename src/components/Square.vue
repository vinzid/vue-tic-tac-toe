<template>
  <button class="square" @click="setValue()">
    {{ value }}
  </button>
</template>

<script>
export default {
  name: 'Square',
  props: ['square', 'value'],
  methods: {
    setValue() {
      this.$emit('click', this.square);
    }
  }
}
</script>