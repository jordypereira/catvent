<template>
  <section class="container min-h-screen flex justify-center items-center text-center mx-auto">
    <div>
      <h1 class="title">catvent</h1>
      <h2 class="subtitle">An advent calendar with cats.</h2>
      <div v-if="fetchedFromLocalStorage" class="catvent-squares flex flex-wrap justify-around">
        <CatventSquare
          class="m-5"
          v-for="i in 25"
          :key="i"
          :id="i"
          :url="catUrlArray[i] ? catUrlArray[i].url : ''"
          :status="catUrlArray[i] ? catUrlArray[i].status : ''"
          :unlockable="unlockable(i)"
          :blacklist="imagesBlacklist"
          @add-cat-url="addCatUrlToCatUrlString"
          @remove-cat-url="removeCatUrlFromCatUrlString"
        />
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
      fetchedFromLocalStorage: false,
      today:  new Date().getDate(),
      catUrlString: '',
      imagesBlacklist: [
        'https://cdn2.thecatapi.com/images/45.gif', 'https://cdn2.thecatapi.com/images/26.gif', 'https://cdn2.thecatapi.com/images/72.gif', 'https://cdn2.thecatapi.com/images/42.gif', 'https://cdn2.thecatapi.com/images/79.gif',
        'https://cdn2.thecatapi.com/images/210.gif',
        'https://cdn2.thecatapi.com/images/98.gif',
        'https://cdn2.thecatapi.com/images/17.gif'
      ],
    }
  },
  computed: {
    catUrlArray() {
      const array =  this.catUrlString.split('_CATURL_')
      const stringArray = array.map(item => item.split('-CATURL-'))
      
      let objectArray = Object.assign({}, 
      ...stringArray.map(item => ({
        [item[0]]: {
          id: item[0],
          url: item[1],
          status: item[2],
        }
      }))
      )
      return objectArray
    },
  },
  methods:{
    createCatUrlString(id, url, status) {
      return `${id}-CATURL-${url}-CATURL-${status}`
    },
    addCatUrlToCatUrlString(id, url, status) {
      const string = this.createCatUrlString(id, url, status)
      this.catUrlString += this.catUrlString !== '' ? `_CATURL_${string}` : string
    },
    removeCatUrlFromCatUrlString(id, url, status) {
      const string = this.createCatUrlString(id, url, status)
      const array = this.catUrlString.split('_CATURL_')
      const updatedArray = array.filter(item => item !== string)
      const updatedString = updatedArray.join('_CATURL_')
      
      this.catUrlString = updatedString
    },
    unlockable(id){
      return (id <= this.today) ? true : false
    },
  },
  watch: {
    catUrlString(string, oldString) {
      localStorage.setItem('catUrl', string)
    }
  },
  async mounted() {
    this.catUrlString = await localStorage.getItem('catUrl') ? localStorage.getItem('catUrl') : ''
    this.fetchedFromLocalStorage = true
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
