<template>
  <div class="board">
    <h1>Jogo da Velha</h1>
    <div class="container">
      <div class="row" v-for="(lines, line) in squares">
        <div class="col-xs-4" v-for="(value, square) in lines" @click="register(line, square, value)">
          <span v-if="value === 1">X</span>
          <span v-else-if="value === 2">O</span>
          <span v-else>&nbsp;</span>
        </div>
      </div>
      <div class="row text-uppercase smFont" v-if="status === 'over'">
        game {{status}} <br> {{msg}} <br>
        <button type="button" class="button btn-primary" @click="reset()">Revanche?</button>
      </div>
      <div class="row" v-if="status !== 'over'">Jogador {{player}}</div>
    </div>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex'

  export default {
    name: 'board',
    computed: mapGetters([
      'squares',
      'player',
      'status',
      'msg'
    ]),
    methods: {
      register (line, square, value) {
        if (value === 0 && this.status !== 'over') {
          this.$store.dispatch('play', { player: this.player, line: line, square: square })
          if (this.status !== 'over') this.$store.dispatch('changePlayer')
        }
      },
      reset () {
        this.$store.dispatch('reset')
      }
    }
  }
</script>

<style lang="scss" scoped>
  $tamanhoQuadro: 250px;
  $border: 2px solid black;
  $geralSize: 50px;
  $otherSize: 20px;

  .container {
    width: $tamanhoQuadro;
    height: $tamanhoQuadro;
    font-size: $geralSize;
    .row:nth-child(-n+2) {
      border-bottom: $border;
    }
    .row {
      div {
        padding-left: 0;
        padding-right: 0;
        &:nth-child(-n+2) {
          border-right: $border;
        }
      }
      span {
        display: block;
        cursor: pointer;
      }
    }
  }
  .smFont {
    font-size: $otherSize;
  }
</style>
