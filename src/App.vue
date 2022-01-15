<template>
  <div id="app">
    <h1>wortel 🥕</h1>
    <div class="guesses">
      <div v-for="(guess, i) in guesses" v-bind:key="i" class="guess">
        <div v-for="(letter, j) in guess" v-bind:key="j" class="word">
          <span v-if="letter.m === 'f'" class="letter full">{{letter.l}}</span>
          <span v-if="letter.m === 'p'" class="letter partial">{{letter.l}}</span>
          <span v-if="letter.m === 'n'" class="letter none">{{letter.l}}</span>
        </div>
      </div>
      <div v-for="index in remaining" :key="index" class="guessplaceholder"></div>
    </div>
    <div class="input">
      <input type="text" v-model="newguess">
      <button>OK</button>
    </div>
    <div class="fix">Für A. :-)</div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data: function () {
    // Sol: Nacht
    // Try: Angst
    return {
      guesses: [
        [{ l: 'n', m: 'p' }, { l: 'a', m: 'p', id: '2' }, { l: 'c', m: 'n' }, { l: 'h', m: 'n' }, { l: 't', m: 'f' }],
        [{ l: 'a', m: 'f' }, { l: 'n', m: 'f', id: '2' }, { l: 'e', m: 'n' }, { l: 'g', m: 'p' }, { l: 't', m: 'f' }],
        [{ l: 'a', m: 'f' }, { l: 'n', m: 'f', id: '2' }, { l: 'g', m: 'f' }, { l: 'e', m: 'n' }, { l: 't', m: 'f' }]
        //        [{ l: 'a', m: 'f' }, { l: 'n', m: 'f', id: '2' }, { l: 'g', m: 'f' }, { l: 'l', m: 'n' }, { l: 't', m: 'f' }]
        //        [{ l: 'a', m: 'f' }, { l: 'n', m: 'f', id: '2' }, { l: 'g', m: 'f' }, { l: 's', m: 'f' }, { l: 't', m: 'f' }]
      ],
      solution: ['angst'],
      newguess: ''
    }
  },
  computed: {
    remaining: function () {
      return 5 - this.guesses.length
    }
  },
  components: {
  }
}
</script>

<style>
#app {
  height: 100vh;
  width: 100vw;
  padding: 0.5em;
  box-sizing: border-box;
  background: #c1d8e6d1;
}
.full {
  background-color: #5E8C61;
}
.partial {
  background-color: #B36D0E;
}
.none {
  background-color: #001524;
}
.letter {
  font-size: 2em;
  color: #fdfffc;
}
.word {
  display: inline-flex;
  flex-direction: column;
}
.letter {
  height: 2em;
  width: 2em;
  display: flex;
  align-items: center;
  justify-content: center;
  text-transform: uppercase;
  font-family: Helvetica;
}
.letter {
  border: solid 1px #fdfffc;
  border-right: none;
  border-top: none;
}
.guess:first-of-type .letter {
  border-top: solid 1px #fdfffc;
}
.guesses {
  text-align: center;
}
.guessplaceholder {
  height: 4em;
}
.input {
  margin: auto;
  width: 20em;
  display: flex;
}
.input input {
  flex-grow: 1;
  margin-right: 2px;
  font-size: 2em;
  flex-grow: 1;
  text-transform: uppercase;
  border: none;
  width: 5em;
  outline: none;
  letter-spacing: 0.5em;
  color: #fdfffc;
  background: #90676C;
  padding-left: 0.5em;
  font-style: italic;
  text-align: center;
}
.input button {
  height: 4em;
  width: 4em;
  border: none;
  background: #7B6D8D;
  color: #fdfffc;
}
.input button:active {
  background: #624882;
}

h1 {
  color: #5E8C61;
  font-family: Helvetica;
  text-align: center;
}
html, body {
  margin: 0;
}
.fix {
  position: fixed;
  bottom: 0px;
  font-size: 0.8em;
}
</style>
