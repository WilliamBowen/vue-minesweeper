<template>
  <div class="square"
    @click="reveal"
    @contextmenu.prevent="rightClicked"
    v-bind:class="{wasClicked: value != '' && value != '@', flagged: flagged}">
    {{ value }}
  </div>
</template>

<script>
export default {
  props: [
    'x',
    'y',
    'value'
  ],
  data() {
    return {
      flagged: false
    }
  },
  created () {
    console.log(this.x, this.y)
  },
  methods: {
    reveal: function () {
      this.$emit('clicked', {x: this.x, y: this.y});
    },
    rightClicked: function () {
      this.flagged = !this.flagged;
    }

  }
}
</script>

<style scoped>
  .square {
    height: 25px;
    width: 25px;
    background-color: grey;
    border: 1px solid black;
    display: inline-block;
    color: transparent;

  }
  .wasClicked {
    background-color: white;
    color: black;
  }
  .flagged {
    background-color: green;
  }

</style>
