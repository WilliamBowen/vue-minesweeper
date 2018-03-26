<template>
  <div class="board">
    <div class="row" v-for="n in height">
      <app-square
        v-for="m in width"
        :key="(n-1)*height + m-1"
        :x="m-1"
        :y="n-1"
        :value="board[(n-1)*height + (m-1)]"
        @clicked="clicked($event)">
      </app-square>
    </div>
    <h1 v-if="gameOver">Game Over</h1>
  </div>
</template>

<script>
import Square from './square.vue';

export default {
  components: {
    'app-square': Square
  },
  data () {
    return {
      board: [],
      mineCount: 99,
      height: 16,
      width: 30,
      gameOver: false
    }
  },
  created : function () {
    let array = [];
    //generate blank board
    for (let i = 0; i < this.width * this.height; i++) {
      array.push("");
    }
    // place mines on board
    for(let mines = 0; mines < this.mineCount; mines++) {
      let x = Math.floor(Math.random() * this.width);
      let y = Math.floor(Math.random() * this.height);
      console.log(x, y);
      if (array[x*this.height + y] != 0) {
        mines--;
        continue;
      }
      array[x*this.height + y] = "@";
    }
    console.log(array)
    this.board = array;
  },
  methods: {
    clicked: function (location) {
      console.log(this.board[location.y*this.height + location.x])
      if(this.board[location.y*this.height + location.x] == "@") {
        this.$set(this.board, location.y*this.height + location.x, 'x');
        this.gameOver = true;
      } else {
        this.reveal(location);
      }
    },
    reveal: function (location) {
      console.log(location)
      let sum = 0;
      // if((location.x > 0) && (location.y > 0) && (this.board[(location.y-1)*this.height + (location.x-1)] == "@")) sum++;
      // if((location.x > 0) && (location.y + 1 < this.height) && (this.board[(location.y+1)*this.height + (location.x-1)] == "@")) sum++;
      // if((location.x > 0) &&(this.board[(location.y)*this.height + (location.x-1)] == "@")) sum++;
      // if((location.x + 1 < this.width) && (location.y > 0) && (this.board[(location.y-1)*this.height + (location.x+1)] == "@")) sum++;
      // if((location.x + 1 < this.width) && (location.y + 1 < this.height) && (this.board[(location.y+1)*this.height + (location.x+1)] == "@")) sum++;
      // if((location.x + 1 < this.width) && (this.board[(location.y)*this.height + (location.x+1)] == "@")) sum++;
      // if((location.y > 0) && (this.board[(location.y-1)*this.height + (location.x)] == "@")) sum++;
      // if((location.y + 1 < this.height) && (this.board[(location.y+1)*this.height + (location.x)] == "@")) sum++;
      for (let i=-1; i<=1; i++) {
        for (let j=-1; j<=1; j++) {
            if (i==0&&j==0) continue;
            if (location.x + i >= 0 && location.x + i < this.width &&
              location.y + j >= 0 && location.y + j < this.height &&
              this.board[(location.y +j)*this.height + location.x + i]== "@")
              sum++;
        }
      }
      if (sum != 0) {
        this.$set(this.board, location.y*this.height + location.x, sum);
        console.log("setting (" + location.x + "," + location.y + ") to " + sum);
      } else {
        this.$set(this.board, location.y*this.height + location.x, " ");
        if((location.x > 0) && (location.y > 0) && (this.board[(location.y-1)*this.height + (location.x-1)] == "")) this.reveal({x: location.x-1, y: location.y-1});
        if((location.x > 0) && (location.y + 1 < this.height) && (this.board[(location.y+1)*this.height + (location.x-1)] == "")) this.reveal({x: location.x-1, y: location.y+1});
        if((location.x > 0) &&(this.board[(location.y)*this.height + (location.x-1)] == "")) this.reveal({x: location.x-1, y: location.y});
        if((location.x + 1 < this.width) && (location.y > 0) && (this.board[(location.y-1)*this.height + (location.x+1)] == "")) this.reveal({x: location.x+1, y: location.y-1});
        if((location.x + 1 < this.width) && (location.y + 1 < this.height) && (this.board[(location.y+1)*this.height + (location.x+1)] == "")) this.reveal({x: location.x+1, y: location.y+1});
        if((location.x + 1 < this.width) && (this.board[(location.y)*this.height + (location.x+1)] == "")) this.reveal({x: location.x+1, y: location.y});
        if((location.y > 0) && (this.board[(location.y-1)*this.height + (location.x)] == "")) this.reveal({x: location.x, y: location.y-1});
        if((location.y + 1 < this.height) && (this.board[(location.y+1)*this.height + (location.x)] == "")) this.reveal({x: location.x, y: location.y+1});

      }
    }
  }
}
</script>

<style scoped>
  .board {
    font-size: 20px;
    text-align: center;
    vertical-align: middle;
    display: block;
  }
  .row {
    display: flex;
  }
</style>
