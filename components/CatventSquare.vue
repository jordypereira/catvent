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
import CatFace from '../components/CatFace';
import axios from 'axios';

export default {
  name: 'CatventSquare',
  props: {
    id: Number,
    today: Number,
    url: String,
    status: String,
    unlockable: Boolean,
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
    preloaded() {
      return (this.status == 'loaded' || this.status == 'preloaded') ? true : false
    },
    loaded() {
      return (this.status == 'loaded') ? true : false
    },
  },
  methods: {
    async turnCard() {
      if (this.unlockable && !this.loaded) {
        // if (!this.preloaded) {
        //   await this.fetchCatImage()
        // }
        // this.setCatCookie(this.catUrl)
        this.$emit('add-cat-url', this.id, this.url, 'loaded')
        // this.removeCatCookie('active-')
        this.$emit('remove-cat-url', this.id, this.url, 'preloaded')
        this.squareLoaded = true
      }
    },
    async fetchCatImage() {
      axios.defaults.headers.common['x-api-key'] = this.apiKey
      try {
        let image = await axios.get('/v1/images/' + this.randomCatUrl)
        // this.setCatCookie(image.data.url, 'active-')
        this.$emit('add-cat-url', this.id, image.data.url, 'preloaded')
        // this.catUrl = image.data.url
        // return image.data.url
      } catch (error) {
        try {
          console.log('Fetching image failed. Trying another image...')
          const imageId = this.randomCatUrl + 1
          let image = await axios.get('/v1/images/' + imageId)
          // this.setCatCookie(image.data.url, 'active-')
          this.$emit('add-cat-url', this.id, image.data.url, 'preloaded')
          // this.catUrl = image.data.url
          // return image.data.url
        } catch (error) {
          try {
            console.log('Fetching image failed. Trying another image...')
            const imageId = this.randomCatUrl + 2
            let image = await axios.get('/v1/images/' + imageId)
            // this.setCatCookie(image.data.url, 'active-')
            this.$emit('add-cat-url', this.id, image.data.url, 'preloaded')
            // this.catUrl = image.data.url
            // return image.data.url
          } catch (error) {
            return console.log('Fetching image failed. Please try Refreshing the page.')
          }
        }
      }
    },
    setCatCookie(catUrl, status = '') {
      this.$cookies.set(status + 'catUrl-' + this.id, catUrl, {
          path: '/',
          maxAge: 60 * 60 * 24 * (26 - this.today)
        })
    },
    removeCatCookie(status = '') {
      this.$cookies.remove(status + 'catUrl-' + this.id, {
          path: '/'
        })
    },
  },
  mounted() {
    axios.defaults.baseURL = 'https://api.thecatapi.com/'
    // if (this.$cookies.get('catUrl-' + this.id)) {
    //   this.catUrl = this.$cookies.get('catUrl-' + this.id, 'loaded')
    //   this.$emit('add-cat-url', this.id, this.catUrl, 'loaded')
    //   this.squareLoaded = true
    // } else if (this.$cookies.get('active-catUrl-' + this.id)) {
    //   this.catUrl = this.$cookies.get('active-catUrl-' + this.id)
    //   this.$emit('add-cat-url', this.id, this.catUrl, 'preloaded')
    // } else 
    if (!this.loaded && !this.preloaded && this.unlockable) {
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
