<template>
  <v-container>
      <v-row v-for="n in 5" :key="n + 'guess'" class="ma-0 pa-0">
        <v-col v-for="i in guesses.length" :key="i" class="pa-1">
          <div class="square" :style="guesses[n-1].attempts[i-1].state == 'correct' ? 'background-color: #538d4e' : guesses[n-1].attempts[i-1].state == 'partial' ? 'background-color: #b59f3b' : guesses[n-1].attempts[i-1].state == 'wrong' ? 'background-color: grey' : ''">
            {{ guesses[n-1].attempts[i-1].key }}
          </div>
        </v-col>
      </v-row>
  </v-container>
</template>

<script>
export default {
  props: {
    guesses: {},
  },
  data: () => ({
   
  }),
};
</script>
<style scoped>
.square {
  border: 2px solid rgb(112, 107, 107);
  height: 8vh;
  font-size: 40px;
  font-weight: bold;
  display: flex;
  justify-content: center;
  align-items: center;
}

</style>