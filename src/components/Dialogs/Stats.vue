<template>
  <v-row justify="center">
    <v-dialog v-model="dialog" max-width="290">
      <v-card>
        <v-card-title class="justify-center">
          STATISTICS
        </v-card-title>

        <v-card-text>

        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

        </v-card-actions>
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
    return {};
  },
};
</script>