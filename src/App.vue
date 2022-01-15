<template>
  <div id="app">
    <h1>wortel <span v-for="index in remaining" :key="index">🥕</span></h1>

    <div class="guesses">
      <div v-for="guess in guesses" :key="guess.id" class="guess">
        <div v-for="letter in guess.guesses" v-bind:key="letter.id" class="word">
          <span v-if="letter.m === 'f'" class="letter full">{{letter.l}}</span>
          <span v-if="letter.m === 'p'" class="letter partial">{{letter.l}}</span>
          <span v-if="letter.m === 'n'" class="letter none">{{letter.l}}</span>
        </div>
      </div>
      <div v-for="index in remaining" :key="index" class="guess placeholder">
       <div class="word">
         <span class="letter">?</span>
       </div>
       <div class="word">
         <span class="letter">?</span>
       </div>
       <div class="word">
         <span class="letter">?</span>
       </div>
       <div class="word">
         <span class="letter">?</span>
       </div>
       <div class="word">
         <span class="letter">?</span>
       </div>
      </div>
    </div>
    <div class="info">
      <span v-if="remaining === 0">Lösung: {{this.solution}}</span>
      <span v-else>{{ info }}</span>
    </div>
    <div class="input">
      <input type="text" v-model="newguess" :disabled="remaining === 0 || won">
      <button @click="check">OK</button>
    </div>
    <div class="fix">Für A. :-)</div>
  </div>
</template>

<script>
import wordlist from './fuenfer.js'
import wordlistExtended from './fuenfer-ext.js'
const randomnr = Math.random() * 1e17
const randomIndex = randomnr % wordlist.length
const randomWord = wordlist[randomIndex]

console.log('No cheating! :-)')
console.log(`Random word is ${randomWord}`)

var checklist = wordlistExtended.concat(wordlist)

export default {
  name: 'App',
  data: function () {
    return {
      guesses: [
      ],
      solution: randomWord,
      newguess: '',
      info: 'Versuch dein Glück',
      won: false
    }
  },
  methods: {
    check: function check () {
      this.info = ''
      if (this.newguess.length < 5) {
        this.info = 'Dein Wort ist zu kurz :-('
        return
      }
      if (this.newguess.length > 5) {
        this.info = 'Dein Wort ist zu lang :-('
        return
      }
      if (checklist.findIndex(w => w.toLowerCase() === this.newguess.toLowerCase()) < 0) {
        this.info = 'Dein Wort ist leider nicht im Korpus'
        return
      }
      var checkResult = {
        id: Math.random() * 1e17,
        guesses: this.newguess.toLowerCase().split('').map(checkWord.bind(this))
      }
      function checkWord (character, index) {
        var result = {
          l: character,
          m: 'n',
          id: '' + Math.random() * 1e17
        }
        var indexOf = this.solution.toLowerCase().indexOf(character)
        window.app = this
        var partialMatch = indexOf >= 0
        if (partialMatch) {
          result.m = 'p'
        }
        var solutionCharAtThisPosition = this.solution.toLowerCase()[index]
        var fullMatch = solutionCharAtThisPosition === character
        if (fullMatch) {
          result.m = 'f'
        }
        return result
      }
      var won = checkResult.guesses.every(character => character.m === 'f')
      if (won) {
        this.info = 'Well done!🥕'
        this.won = won
      } else {
        this.info = 'Fast :-)'
      }

      this.guesses.push(checkResult)
      this.newguess = ''
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
  font-size: 1.5em;
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
.word:last-of-type .letter {
  border-right: solid 1px #fdfffc;
}
.guess:first-of-type .letter {
  border-top: solid 1px #fdfffc;
}
.guesses {
  text-align: center;
  margin-bottom: 1em;
}
.input {
  margin: auto;
  width: 20em;
  display: flex;
}
.input input {
  flex-grow: 1;
  margin-right: 2px;
  font-size: 1.5em;
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
.info {
  text-align: center;
  font-size: 1em;
  padding: 0.5em;
  font-family: Helvetica;
}
</style>
