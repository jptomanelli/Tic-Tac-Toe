<template>
  <div id="app">
    <h1>Tic Tac Toe</h1>
    <board></board>
    <p>Game: {{ games }}</p>
    <button v-on:click="restart">Restart</button>
  </div>
</template>

<script>
import Board from './components/Board.vue'
export default {
  components: { Board },

  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      games: 1,
      xWins: 0,
      oWins: 0,
    }
  },

  methods: {
    restart() {
      Event.$emit('clearCell')
      Event.$emit('boardReset')

      this.games++
    }
  }
}
</script>

<style>
body {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
  background-color: #FFFFF1;
}

#app {
  margin: 0 auto;
  max-width: 270px;
  border-top-left-radius: 20px;
  border-top-right-radius: 20px;
  border-bottom-left-radius: 20px;
  border-bottom-right-radius: 20px;
  color: black;
}

button {
  color: white;
  width: 100px;
  height: 35px;
  font-size: 1em;
  font-align: center;
  border: solid #222 0.2rem;
  border-radius: 5px;
  cursor: pointer;
  background-color: gray;
  box-shadow: 4px 4px 0 #222;
  margin-bottom: 1rem;
}

h1 {
  font-weight: normal;
  font-size: 50px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
