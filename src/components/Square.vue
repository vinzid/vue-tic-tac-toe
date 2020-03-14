<template>
  <button class="square" @click="setValue()">
    {{ value }}
  </button>
</template>

<script>
export default {
  name: 'Square',
  //props: ['value'],
  data() {
    return {
      value: null
    }
  },
  methods: {
    setValue() {
      this.value = 'X';
    }
  }
}
</script>