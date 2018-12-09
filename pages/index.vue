<template>
  <section class="container min-h-screen flex justify-center items-center text-center mx-auto">
    <div>
      <h1 class="title">catvent</h1>
      <h2 class="subtitle">An advent calendar with cats.</h2>
      <div class="catvent-squares flex flex-wrap justify-around">
        <CatventSquare
          class="m-5"
          v-for="i in 25"
          :key="i"
          :id="i"
          :url="catUrlArray[i] ? catUrlArray[i].url : ''"
          :status="catUrlArray[i] ? catUrlArray[i].status : ''"
          :unlockable="unlockable(i)"
          :today="today"
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
      today:  new Date().getDate(),
      catUrlString: '',
      imagesBlacklist: ['https://cdn2.thecatapi.com/images/45.gif'],
    }
  },
  computed: {
    catUrlArray() {
      const array =  this.catUrlString.split('_')
      const stringArray = array.map(item => item.split('-'))
      
      let objectArray = Object.assign({}, 
      ...stringArray.map(item => ({
        [item[1]]: {
          id: item[1],
          status: item[0],
          url: item[2],
        }
      }))
      )
      return objectArray
    },
  },
  methods:{
    addCatUrlToCatUrlString(id, catUrl, status) {
      const string = status ? `${status}-${id}-${catUrl}` : `${id}-${catUrl}`
      this.catUrlString += this.catUrlString !== '' ? `_${string}` : string
    },
    removeCatUrlFromCatUrlString(id, catUrl, status) {
      const string = status ? `${status}-${id}-${catUrl}` : `${id}-${catUrl}`
      const array = this.catUrlString.split('_')
      const updatedArray = array.filter(catUrl => catUrl !== string)
      const updatedString = updatedArray.join('_')
      
      this.catUrlString = updatedString
    },
    catUrlObject(string) {
      const array = string.split('-')
      return  {
        status: array[0],
        url: array[2],
      }
    },
    unlockable(id){
      return (id <= this.today) ? true : false
    },
  },
  watch: {
    catUrlString(string, oldString) {
      console.log('Updated local storage')
      localStorage.setItem('catUrl', string)
    }
  },
  mounted() {
    this.catUrlString = localStorage.getItem('catUrl') ? localStorage.getItem('catUrl') : ''
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
