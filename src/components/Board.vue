<template>
<div>
	<div class="gameStatus">
		{{ gameStatusMessage }}
	</div>
	<div>
		<table class="board">
			<tr>
				<cell name="0"></cell>
				<cell name="1"></cell>
				<cell name="2"></cell>
			</tr>
				<cell name="3"></cell>
				<cell name="4"></cell>
				<cell name="5"></cell>
			<tr>
				<cell name="6"></cell>
				<cell name="7"></cell>
				<cell name="8"></cell>
			</tr>
		</table>
	</div>
</div>
</template>

<script>
import Cell from './Cell.vue'

export default {
	components: { Cell },

	data() {
		return {
			playerTurn: 'X',
			gameStatus: 'turn',
			gameStatusMessage: 'Xs turn',
			moves: 0,
			cells: {
				0: '', 1: '', 2: '',
				3: '', 4: '', 5: '',
				6: '', 7: '', 8: '',
			},
			winningPos: [
				[0,1,2],[3,4,5],[6,7,8],
				[0,3,6],[1,4,7],[2,5,8],
				[0,4,8],[2,4,6]
			],
		}
	},

	// In-template expressions
	computed: {
		otherPlayer() {
			if (this.playerTurn === 'X') { return 'O' }
			return 'X'
		}
	},

	watch: {
		gameStatus() {
			if (this.gameStatus === 'win') {
				this.gameStatusMessage = 'Winner!'
				return
			} else if (this.gameStatus === 'draw') {
				this.gameStatusMessage = "Draw!"
				return
			}
			this.gameStatusMessage = `${this.playerTurn}s turn`
		}
	},

	methods: {

		changePlayer() {
			this.playerTurn = this.otherPlayer;
		},

		checkWin() {

			for (let i = 0; i < this.winningPos.length; i++) {

				let pos = this.winningPos[i]
				let cells = this.cells

				if (this.areEqual(cells[pos[0]], cells[pos[1]], cells[pos[2]])) {
					return true
				}
			}
			return false
		},

		changeStatus() {
			if (this.checkWin()) {
				this.gameStatusMessage = `${this.playerTurn} Wins!`
				Event.$emit('fill');
				return 'win'
			} else if (this.moves === 9) {
				return 'draw'
			}
			this.gameStatusMessage = `${this.playerTurn}s turn`
			return 'turn'
		},

		areEqual() {
			let length = arguments.length

			if (arguments[0] === arguments[1] &&
				arguments[1] === arguments[2] &&
				arguments[2] !== '') {
				return true
			}
			return false
		},

	},

	created() {
		Event.$on('mark', (num) => {
			this.cells[num] = this.playerTurn
			this.moves++
			this.changePlayer()
			this.gameStatus = this.changeStatus()
		})

		Event.$on('boardReset', () => {
			Object.assign(this.$data, this.$options.data())
		})
	}
}
</script>

<style>
.grid {
	background-color: white;
	color: black;
	width: 80%;
	boarder-collapse: collapse;
	font-align: center;
}

.gameStatus {
	margin: 0px;
	padding: 10px;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  color: black;
  font-size: 1.4em;
	max-width: 100%;
}
</style>
