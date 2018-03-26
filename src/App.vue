<template>
  <div id="app">
    <app-board></app-board>
  </div>
</template>

<script>
  import Board from "./components/board.vue";

export default {
  components: {
    'app-board': Board
  },
  name: 'app',
  data () {
    return {

    }
  }
}
</script>

<style lang="scss">
  
</style>
