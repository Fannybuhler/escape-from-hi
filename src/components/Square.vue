<template>
  <button class="square" @click="fillSquare">{{ value }}</button>
</template>

<script>
import { mapGetters, mapState} from 'vuex'

export default {
  name: 'Square',
  props: {
    position: Number,
    value: String
  },

  computed: {
    ...mapState([
      'isStarted'
    ]),
    ...mapGetters({
      squares: 'getSquares',
      currentPlayer: 'getCurrentPlayer',
      stepNumber: 'getStepNumber',
      winner: 'getWinner',
    }),
  },

  methods: {
    fillSquare() {
      if(this.isStarted && this.currentPlayer === 'X') {
        const squareValue = this.squares[this.position]

        if(this.winner) {
          return
        }

        if (squareValue === null) {
          this.$store.dispatch('clickedSquare', this.position)
          this.dispatchers()
        }

        this.opponentMove()
        return
      }
    },

    opponentMove() {
      let arr = this.squares
      let index = Math.floor(Math.random() * arr.length)
      while ((arr[index] === 'X') || (arr[index] === 'O')) {
        index = Math.floor(Math.random() * arr.length)
        console.log('Picked random index: ', index)
        if(this.winner || this.stepNumber > 8) {
          return
        }
      }
      this.$store.dispatch('clickedSquare', index)
      this.dispatchers()
      console.log('filled index: ', index, 'with O')
    },
 
    dispatchers() {
      this.$store.dispatch('increaseStepNumber')
      this.$store.dispatch('calculateWinner')
      this.$store.dispatch('flipCurrentPlayer')
    }
  }
}
</script>

<style scoped>
.square {
  color: #000;
  background: #ffffff14;
  background-blend-mode: exclusion;
  border: 1px solid #0000008c;
  font-size: 15vmin;
  font-weight: bold;
  line-height: 34px;
  padding: 0;
  text-align: center;
  box-shadow: inset 0 0 0 #0004;
  text-shadow: 0 0 1px #33920e, 0 2px 5px #5dbf38;
  transition: all .25s ease;
  outline: none;
}

.square:not([disabled]):empty:hover,
.square:not([disabled]):empty:focus {
  box-shadow: inset 0 2px 25px #000000ad;
  cursor: pointer;
}
.square:not([disabled]):empty:active {
  box-shadow: inset 0 2px 50px #000000ad;
}
</style>