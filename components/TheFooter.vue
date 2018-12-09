<template>
  <footer class="w-full flex justify-center my-3">
    <p class="mx-2 flex justify-center flex-wrap">
      Follow me on
      <a
        href="https://twitter.com/_perjor"
        class="text-pink-light no-underline hover:text-indigo px-1"
      >Twitter</a>
      <span class="mx-2">-</span>
      Start over by
      <button
        class="text-pink-light no-underline hover:text-indigo px-1"
        @click="resetCats()"
      >Resetting the calendar</button>
    </p>
  </footer>
</template>

<script>
export default {
  name: 'TheFooter',
  methods: {
    resetCats() {
      this.$cookies.removeAll()
      localStorage.clear()
      location.reload(true)
    }
  },
}
</script>

<style>
</style>
