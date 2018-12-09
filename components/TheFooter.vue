<template>
  <footer class="w-full flex justify-center my-3">
    <p>
      Follow me on
      <a
        href="https://twitter.com/_perjor"
        class="text-pink-light no-underline hover:text-indigo"
      >Twitter</a>
    </p>
    <p class="mx-2">-</p>
    <p>
      Start over by
      <button
        class="text-pink-light no-underline hover:text-indigo"
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
      location.reload(true)
    }
  },
}
</script>

<style>
</style>
