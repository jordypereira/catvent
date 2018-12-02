<template>
  <div class="square" :class="{ 'square--active': squareActive }">
    <div class="square__day">{{ id }}</div>
    <CatFace v-if="squareActive"/>
  </div>
</template>

<script>
import CatFace from "../components/CatFace";

export default {
  name: "CatventSquare",
  props: {
    id: Number
  },
  components: {
    CatFace
  },
  computed: {
    squareActive() {
      if (this.id < 7) return true;
      else return false;
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
