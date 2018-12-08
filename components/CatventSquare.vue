<template>
  <div
    class="square"
    :class="{ 'square--active': squareActive, 'square--loaded': squareLoaded }"
    @click="turnCard()"
  >
    <div class="square__day flex justify-center items-center rounded-full bg-pink-lighter">{{ id }}</div>
    <CatFace v-if="!squareLoaded" :isActive="squareActive"/>
    <div v-if="squareActive" v-show="squareLoaded" class="catImage">
      <img :src="catUrl" alt="Cat image">
    </div>
  </div>
</template>

<script>
import CatFace from '../components/CatFace';
import axios from 'axios';

export default {
  name: 'CatventSquare',
  props: {
    id: Number,
    today: Number,
  },
  data() {
    return {
      apiKey: process.env.API_KEY,
      catUrl: '',
      squareLoaded: false,
    };
  },
  components: {
    CatFace,
  },
  computed: {
    squareActive() {
      if (this.id <= this.today) return true
      else return false
    },
    randomCatUrl() {
      const modifier = 30
      const min = (this.id - 1) * modifier
      const max = this.id * modifier
      return Math.floor(Math.random() * (max - min) + min)
    },
  },
  methods: {
    async turnCard() {
      if (this.squareActive) {
        if (!this.catUrl) {
          await this.fetchCatImage()
        }
        this.setCatCookie(this.catUrl)
        this.squareLoaded = true
      }
    },
    async fetchCatImage() {
      axios.defaults.headers.common['x-api-key'] = this.apiKey
      try {
        let image = await axios.get('/v1/images/' + this.randomCatUrl)
        this.setCatCookie(image.data.url, 'active-')
        this.catUrl = image.data.url
        return image.data.url
      } catch (error) {
        return console.log('Fetching image failed. Please try Refreshing the page.')
      }
    },
    setCatCookie(catUrl, status = '') {
      this.$cookies.set(status + 'catUrl-' + this.id, catUrl, {
          path: '/',
          maxAge: 60 * 60 * 24 * (26 - this.today)
        })
    },
  },
  mounted() {
    axios.defaults.baseURL = 'https://api.thecatapi.com/'
    if (this.$cookies.get('catUrl-' + this.id)) {
      this.catUrl = this.$cookies.get('catUrl-' + this.id)
      this.squareLoaded = true
    } else if (this.$cookies.get('active-catUrl-' + this.id)) {
      this.catUrl = this.$cookies.get('active-catUrl-' + this.id)
    } else if (this.squareActive) {
      this.fetchCatImage()
    }
  },
}
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

.square--active {
  transition: background 0.1s;
}

.square--active:hover {
  cursor: pointer;
  @apply bg-indigo-light;
}

.square--loaded {
  background: none;
}

.square--loaded:hover {
  background: none;
  cursor: default;
}

.square__day {
  position: absolute;
  top: var(--day-pos);
  left: var(--day-pos);
  width: var(--day-size);
  height: var(--day-size);
  font-weight: 700;
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
