<template>
  <v-app :style="{ background: '#141414' }">
    <v-app-bar app color="#141414">
      <v-icon right> mdi-menu </v-icon>
      <v-icon right> mdi-help-circle-outline </v-icon>
      <v-spacer></v-spacer>
      <h3 style="font-size: 25px">Wordle</h3>
      <v-spacer></v-spacer>
      <v-icon right @click="openDialog('stats')"> mdi-poll </v-icon>
      <v-icon right> mdi-cog </v-icon>
    </v-app-bar>

    <v-main>
    <Main @openDialog="openDialog"></Main>
    <Stats :dialog="statsDialog" />
    </v-main>

  </v-app>
</template>

<script>
import Main from './components/Main.vue'
import Stats from './components/Dialogs/Stats.vue'
export default {
  components: {
    Main,
    Stats
  },
  data: () => ({
    statsDialog: false,
  }),
  methods: {
    openDialog(type){
    debugger
      if(type == "stats"){
        this.statsDialog = true;
      }
    }
  }
};
</script>
<style scoped>
</style>
