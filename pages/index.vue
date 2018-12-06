<template>
  <section class="container">
    <div>
      <button
        class="absolute pin-r m-5 px-5 py-2 bg-pink-lighter hover:bg-pink-lightest rounded-full"
        @click="resetCats()"
      >Reset</button>
      <h1 class="title">catvent</h1>
      <h2 class="subtitle">An advent calendar with cats.</h2>
      <div class="catvent-squares flex flex-wrap justify-around">
        <CatventSquare v-for="i in 24" :key="i" class="m-5" :id="i"/>
      </div>
    </div>
  </section>
</template>

<script>
import CatventSquare from "../components/CatventSquare";

export default {
  components: {
    CatventSquare
  },
  methods: {
    resetCats() {
      this.$cookies.removeAll();
      location.reload(true);
    }
  }
};
</script>

<style>
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}

/* .container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  margin: 0 auto;
} */

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}
</style>
