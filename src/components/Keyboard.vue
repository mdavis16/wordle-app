<template>
  <v-container class="pl-1 pr-1">
    <div style="height: 100%">
      <v-row
        v-for="n in 3"
        :key="n + 'key'"
        :class="n != 2 ? 'ma-0 pl-1 pr-1' : 'ma-0 pl-5 pr-5'"
      >
        <v-col
          v-for="i in keys[n - 1].keySet.length"
          :key="i"
          style="padding: 2px"
        >
          <div class="key pa-1" @click="onKeyPress(keys[n - 1].keySet[i - 1])">
            {{ keys[n - 1].keySet[i - 1] }}
          </div>
        </v-col>
      </v-row>
    </div>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    keys: [
      { keySet: ["Q", "W", "E", "R", "T", "Y", "U", "I", "O", "P"] },
      { keySet: ["A", "S", "D", "F", "G", "H", "J", "K", "L"] },
      { keySet: ["ENTER", "Z", "X", "C", "V", "B", "N", "M", "BACK"] },
    ],
  }),
  methods:
  {
    onKeyPress(key){
      this.$emit('onKeyPress',key);
    }
  }
};
</script>
<style scoped>
.key {
  background-color: gray;
  height: 8vh;
  border-radius: 5px;
  font-weight: bold;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>