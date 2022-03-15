<template>
  <v-app :style="{ background: '#141414' }">
    <v-app-bar app color="#141414">
      <v-icon right> mdi-menu </v-icon>
      <v-icon right> mdi-help-circle-outline </v-icon>
      <v-spacer></v-spacer>
      <h3 style="font-size: 25px">Wordle</h3>
      <v-spacer></v-spacer>
      <v-icon right> mdi-poll </v-icon>
      <v-icon right> mdi-cog </v-icon>
    </v-app-bar>

    <v-main>
    <Main></Main>
    </v-main>

  </v-app>
</template>

<script>
import Main from './components/Main.vue'
export default {
  components: {
    Main
  },
  data: () => ({
  
  }),
};
</script>
<style scoped>
</style>
