<template>
  <section class="container min-h-screen flex justify-center items-center text-center mx-auto">
    <div>
      <h1 class="title">catvent</h1>
      <h2 class="subtitle">An advent calendar with cats.</h2>
      <button
        class="m-5 px-5 py-2 bg-pink-lighter hover:bg-pink-lightest rounded-full"
        @click="resetCats()"
      >Reset</button>
      <div class="catvent-squares flex flex-wrap justify-around">
        <CatventSquare v-for="i in 25" :key="i" class="m-5" :id="i" :today="today"/>
      </div>
    </div>
  </section>
</template>

<script>
import CatventSquare from '../components/CatventSquare'

export default {
  name: 'Catvent-Page',
  components: {
    CatventSquare,
  },
  data() {
    return {
      today:  new Date().getDate(),
    }
  },
  methods: {
    resetCats() {
      this.$cookies.removeAll()
      location.reload(true)
    }
  },
}
</script>

<style>
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
