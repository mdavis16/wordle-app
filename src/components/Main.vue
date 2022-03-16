<template>
  <v-container>
    <v-row align="center" class="ma-0 pa-0" justify="center">
      <Gameboard :guesses="guesses" />
    </v-row>
    <v-row align="end" class="ma-0 pa-0" justify="center">
      <Keyboard @onKeyPress="onKeyPress" />
    </v-row>

    <v-snackbar
      :style="{ 'margin-bottom': calcMargin(i) }"
      v-for="(s, i) in snackbars"
      centered
      :key="i"
      v-model="showSnackBars"
    >
      {{ s.text }}
    </v-snackbar>

    <Stats :dialog="statsDialog" />
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
  }),

  methods: {
    onKeyPress(key) {
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
      if (this.index > 0) {
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
      debugger;
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

      if (count == 5) {
        this.endGame(true);
        return;
      }

      this.activeGuess++;
    },

    endGame(win) {
      this.statsDialog = true;
    },

    getGuessWord() {
      var word = "";
      this.guesses[this.activeGuess].attempts.map((x) => (word += x.key));
      return word;
    },

    calcMargin(i) {
      if (i != 0) {
        return i * -62 + "px";
      }
      return;
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