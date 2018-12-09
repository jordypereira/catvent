<template>
  <div
    class="square"
    :class="{ 'square--active': unlockable, 'square--loaded': loaded }"
    @click="turnCard()"
  >
    <div class="square__day flex justify-center items-center rounded-full bg-pink-lighter">{{ id }}</div>
    <CatFace v-if="!loaded" :isActive="unlockable"/>
    <div v-if="preloaded" v-show="loaded" class="square__image">
      <img :src="url" alt="Cat image">
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import CatFace from '../components/CatFace';

export default {
  name: 'CatventSquare',
  props: {
    id: Number,
    url: String,
    status: String,
    unlockable: Boolean,
    blacklist: Array,
  },
  data() {
    return {
      apiKey: process.env.API_KEY,
      tries: 0,
    };
  },
  components: {
    CatFace,
  },
  computed: {
    randomCatUrl() {
      const modifier = 30
      const min = (this.id - 1) * modifier
      const max = this.id * modifier
      return Math.floor(Math.random() * (max - min - 5) + min)
    },
    preloaded() {
      return (this.status == 'loaded' || this.status == 'preloaded') ? true : false
    },
    loaded() {
      return (this.status == 'loaded') ? true : false
    },
  },
  methods: {
    async turnCard() {
      if (this.preloaded && !this.loaded) {
        this.$emit('add-cat-url', this.id, this.url, 'loaded')
        this.$emit('remove-cat-url', this.id, this.url, 'preloaded')
      }
    },
    async fetchCatImage() {
      axios.defaults.headers.common['x-api-key'] = this.apiKey
      if (this.tries < 20) {
        try {
          const imageId = this.randomCatUrl + this.tries
          let image = await axios.get('https://api.thecatapi.com/v1/images/' + imageId)
          if (this.blacklist.includes(image.data.url)) {
            console.log('Fetched image blacklisted. Trying another image...')
            this.tries++
            this.fetchCatImage()
            return false
          }
          this.$emit('add-cat-url', this.id, image.data.url, 'preloaded')
        } catch (error) {
          console.log('Fetching image failed. Trying another image...')
          this.tries++
          this.fetchCatImage()
          return false
        }
      } else {
          return console.log('Fetching image failed. Please try Refreshing the page.')
        }
    },
    removeCatCookie(status = '') {
      this.$cookies.remove(status + 'catUrl-' + this.id, {
          path: '/'
        })
    },
  },
  mounted() {
    if (this.$cookies.get('catUrl-' + this.id)) {
      const url = this.$cookies.get('catUrl-' + this.id)
      this.$emit('add-cat-url', this.id, url, 'loaded')
      this.removeCatCookie()
    } else if (this.$cookies.get('active-catUrl-' + this.id)) {
      const url = this.$cookies.get('active-catUrl-' + this.id)
      this.$emit('add-cat-url', this.id, url, 'preloaded')
      this.removeCatCookie('active-')
    } else if (!this.loaded && !this.preloaded && this.unlockable) {
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
  transition: background 0.3s;
  @apply bg-indigo-lighter;
}

.square--active:hover {
  cursor: pointer;
  background: none;
}

.square--active:hover .ears {
  animation: animateEars 0.5s linear 0s 1;
}

@keyframes animateEars {
  0% {
    transform: rotateX(0deg);
  }
  70% {
    transform: rotateX(-70deg);
  }
  100% {
    transform: rotateX(0deg);
  }
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

.square__image {
  height: 100%;
  display: flex;
  align-items: center;
  align-content: center;
  justify-content: center;
}

.square__image > img {
  max-width: var(--square-size);
  max-height: var(--square-size);
  height: auto;
}
</style>
