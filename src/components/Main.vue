<template>
  <v-container  class="pt-0 pb-0">
    <v-row align="center" class="ma-0 pa-0" justify="center">
      <Gameboard  style="max-width: 500px" :guesses="guesses" />
    </v-row>
    <v-row  align="end" class="ma-0 pa-0" justify="center">
      <Keyboard style="max-width: 500px" @onKeyPress="onKeyPress" />
    </v-row>


  

    <v-snackbar
      :style="{  'top': calcTop(i)}"
      v-for="(s, i) in snackbars"
      app
      color='white'
      :key="i"
      v-model="showSnackBars"
    >
      <p class = "ma-0 pa-0" style="text-align: center; color: black">{{ s.text }}</p>
    </v-snackbar>

  </v-container>
</template>

<script>
import Keyboard from "./Keyboard.vue";
import Gameboard from "./Gameboard.vue";
import Stats from "./Dialogs/Stats.vue";
import dict from "../assets/wordleDict.txt";

export default {

  name: "Main",
  components: {
    Keyboard,
    Gameboard,
    Stats,
  },
  data: () => ({
    guesses: [
      {
        attempts: [
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
        ],
      },
      {
        attempts: [
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
        ],
      },
      {
        attempts: [
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
        ],
      },
      {
        attempts: [
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
        ],
      },
      {
        attempts: [
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
          { key: "", state: "empty" },
        ],
      },
    ],

    word: "SMASH",
    index: 0,
    activeGuess: 0,
    snackbars: [],
    showSnackBars: false,
    statsDialog: false,
    isGameActive: true,
  }),

  methods: {
    onKeyPress(key) {
      if(!this.isGameActive)
        return;

      if (key == "ENTER") {
        this.handleEnter(key);
      } else if (key == "BACK") {
        this.handleBack(key);
      } else {
        this.handleOther(key);
      }
    },

    handleEnter() {
      var guessWord = this.getGuessWord();

      if (guessWord.length < 5) {
        this.showSnackBars = true;
        this.snackbars.push({ text: "Not Enough Letters" });
      } else {
        this.onGuessEnd(guessWord);
      }
    },

    handleBack() {
      if (this.index > 0 && this.isGameActive) {
        this.index--;
        this.guesses[this.activeGuess].attempts[this.index].key = "";
      }
    },

    handleOther(key) {
      if (this.index == 5) {
        return;
      } else {
        this.guesses[this.activeGuess].attempts[this.index].key = key;
        this.index++;
      }
    },

    validateWord(guessWord) {
      return dict.includes(guessWord.toLowerCase());
    },

    onGuessEnd(guessWord) {
      if (!this.validateWord(guessWord)) {
        this.showSnackBars = true;
        this.snackbars.push({ text: "Not In Word List" });
        return;
      }
      var count = 0;
      for (var i = 0; i < guessWord.length; i++) {
        if (guessWord[i] == this.word[i]) {
          this.guesses[this.activeGuess].attempts[i].state = "correct";
          count++;
        } else if (this.word.includes(guessWord[i])) {
          this.guesses[this.activeGuess].attempts[i].state = "partial";
        } else {
          this.guesses[this.activeGuess].attempts[i].state = "wrong";
        }
      }

      //won game 
      if (count == 5) {
        this.endGame(true);
        return;
      } 
      else { 
        this.activeGuess++;
        this.index = 0;
        if(this.activeGuess == 5){
          this.endGame(false);
        }        
      }

    },

    endGame(win) {
      this.isGameActive = false; 
      if(win){
        this.showSnackBars = true;
        this.snackbars.push({ text: "Great Job!" });
      } 
      else {
       this.showSnackBars = true;
       this.snackbars.push({ text: this.word });
      }

      this.$emit('openDialog' , 'stats');
      
    },

    getGuessWord() {
      var word = "";
      this.guesses[this.activeGuess].attempts.map((x) => (word += x.key));
      return word;
    },

    calcMargin(i) {
      if (i != 0) {
        return i * -65 + "px";
      }
      return;
    },
    calcTop(i){
      return -90 + i * 5 + "%";
    },
    hide(i) {
      this.snackbars.splice(i, 1);
    },
  },

  watch: {
    clonedSnackBars: {
      handler: function (newVal, oldVal) {
        if (newVal.length > oldVal.length) {
          var self = this;
          setTimeout(function () {
            self.snackbars.shift();
          }, 1500);
        }
      },
    },
  },
  computed: {
    clonedSnackBars: function () {
      return JSON.parse(JSON.stringify(this.snackbars));
    },
  },
};
</script>
<style scoped>
</style>