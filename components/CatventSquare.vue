<template>
  <div
    class="square"
    :class="{ 'square--active': squareActive, 'square--loaded': catUrl }"
    @click="turnCard()"
  >
    <div class="square__day">{{ id }}</div>
    <CatFace v-if="!catUrl" :isActive="squareActive"/>
    <div v-if="catUrl" class="catImage">
      <img :src="catUrl" alt="Cat image">
    </div>
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
      apiKey: process.env.API_KEY,
      catUrl: this.$cookies.get("catUrl-" + this.id)
        ? this.$cookies.get("catUrl-" + this.id)
        : "",
      today: 1
    };
  },
  components: {
    CatFace
  },
  computed: {
    squareActive() {
      if (this.id <= this.today && !this.catUrl) return true;
      else return false;
    },
    randomCatUrl() {
      const modifier = 30;
      const min = (this.id - 1) * modifier;
      const max = this.id * modifier;
      return Math.floor(Math.random() * (max - min) + min);
    }
  },
  methods: {
    async turnCard() {
      if (this.squareActive) {
        this.catUrl = await this.fetchCatImage();
        this.$cookies.set("catUrl-" + this.id, this.catUrl, {
          path: "/",
          maxAge: 60 * 60 * 24 * (26 - this.today)
        });
      }
    },
    async fetchCatImage() {
      axios.defaults.headers.common["x-api-key"] = this.apiKey;
      let image = await axios.get("/v1/images/" + this.randomCatUrl);

      return image.data.url;
    }
  },
  mounted() {
    axios.defaults.baseURL = "https://api.thecatapi.com/";
    const date = new Date();
    this.today = date.getDate();
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

.square--loaded {
  background: none;
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

.catImage {
  height: 100%;
  display: flex;
  align-items: center;
  align-content: center;
  justify-content: center;
}

.catImage > img {
  max-width: var(--square-size);
  max-height: var(--square-size);
  height: auto;
}
</style>
