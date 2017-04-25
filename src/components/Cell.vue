<template>
	<td class="cell" v-on:click="mark">{{ sym }}</td>
</template>

<script>
	export default {
		props: ['name'],
		data() {
			return {
				filled: false,
				sym: ''
			}
		},

		methods: {
			mark() {
				if (!this.filled) {
					this.sym = this.$parent.playerTurn
					this.filled = true
					Event.$emit('mark', this.name)
				}
			},
		},

		created() {
			Event.$on('clearCell', () => {
				this.sym = ''
				this.filled = false;
			})

			Event.$on('fill', () => this.filled = true )
		}
	}
</script>

<style>
.cell {
	width: 80px;
	height: 80px;
	font-size: 3.5em;
	font-align: center;
	color: #222;
  font-weight: 600;
  border: solid #222 0.2rem;
  border-radius: 5px;
  cursor: pointer;
  background-color: #83ffcd;
  box-shadow: 4px 4px 0 #222;
  margin-bottom: 1rem;
}

.cell:hover {
	 background-color: #83f1cd;
}


</style>
