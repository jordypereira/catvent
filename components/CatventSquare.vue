<template>
  <div class="square" :class="{ 'square--active': squareActive }" @click="turnCard()">
    <div class="square__day">{{ id }}</div>
    <CatFace v-if="squareActive && !squareRevealed"/>
  </div>
</template>

<script>
import CatFace from "../components/CatFace";
import axios from "axios";

export default {
  name: "CatventSquare",
  props: {
    id: Number
  },
  data() {
    return {
      squareRevealed: false,
      apiKey: process.env.API_KEY
    };
  },
  components: {
    CatFace
  },
  computed: {
    squareActive() {
      if (this.id < 7) return true;
      else return false;
    }
  },
  methods: {
    turnCard() {
      if (this.squareActive) {
        this.squareRevealed = true;
      }
    },
    fetchCatImage() {
      axios.get();
    }
  }
};
</script>

<style>
:root {
  --square-size: 200px;
  --day-size: 48px;
  --day-pos: calc(var(--day-size) / -4);
}
.square {
  height: var(--square-size);
  width: var(--square-size);
  position: relative;
  @apply bg-indigo;
}

.square--active:hover {
  cursor: pointer;
  @apply bg-indigo-light;
}

.square__day {
  position: absolute;
  top: var(--day-pos);
  left: var(--day-pos);
  width: var(--day-size);
  height: var(--day-size);
  font-weight: 700;
  @apply flex justify-center items-center rounded-full bg-pink-lighter;
}
</style>
