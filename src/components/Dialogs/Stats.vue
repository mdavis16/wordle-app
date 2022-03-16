<template>
  <v-row justify="center">
    <v-dialog v-model="dialog" max-width="500" >
      <v-card class="pa-1">
        <v-card-title class="justify-center"> STATISTICS </v-card-title>
        <v-row class="text-center">
          <v-col cols="3">1</v-col>
          <v-col cols="3">100</v-col>
          <v-col cols="3">1</v-col>
          <v-col cols="3">1</v-col>
        </v-row>
        <v-row class="text-center mt-n6" style="font-size: 0.8em">
          <v-col cols="3"> Played </v-col>
          <v-col cols="3"> Win % </v-col>
          <v-col cols="3"> Current Streak </v-col>
          <v-col cols="3">Max Streak</v-col>
        </v-row>
        <v-spacer></v-spacer>
        <v-card-title class="justify-center pb-1"
          >GUESS DISTRIBUTION</v-card-title
        >
        <v-row
          align="center"
          justify="center"
          class="mt-0"
          v-for="n in 6"
          :key="n"
        >
          <v-col cols="1">{{ n }}</v-col>
          <v-col cols="10">
            <div
              style="background-color: green; text-align: right; width: 100%"
            >
              1
            </div>
          </v-col>
        </v-row>

        <v-row class="text-center pb-5">
          <v-col cols="6" style="font-size: 20px">
            <pre>Next Wordle</pre>
            {{ hours }}:{{ mins }}:{{ secs }}</v-col
          >
          <v-divider vertical></v-divider>
          <v-col cols="6">
            <v-btn color="success" class="ma-2 white--text">
              Upload
              <v-icon right dark> mdi-share-variant-outline </v-icon>
            </v-btn>
          </v-col>
        </v-row>
      </v-card>
    </v-dialog>
  </v-row>
</template>
<script>
export default {
  props: {
    dialog: Boolean,
  },
  data() {
    return {
      currentDate: new Date(),
      hours: "",
      mins: "",
      secs: "",
    };
  },
  created() {
    this.currentDate.setDate(this.currentDate.getDate() + 1);
    this.currentDate.setHours(0, 0, 0, 0);
    var self = this;
    window.setInterval(() => {
      self.updateTime();
    }, 2000);
  },
  methods: {
    updateTime() {
      var now = new Date().getTime();

      // Find the distance between now and the count down date
      var distance = this.currentDate.getTime() - now;

      // Time calculations for days, hours, minutes and seconds
      var days = Math.floor(distance / (1000 * 60 * 60 * 24));
      var hours = Math.floor(
        (distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
      );
      var minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
      var seconds = Math.floor((distance % (1000 * 60)) / 1000);

      this.hours = hours;
      this.mins = minutes;
      this.secs = seconds;
    },
  },
};
</script>