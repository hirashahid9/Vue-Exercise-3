<template>
  <div id="app">
    <span class=" text-5xl font-bold">Vue.js Exercise</span>
    <h2 class=" text-2xl font-bold">Total coins: {{coins.length}}</h2>
    <button class="bg-gray-700 mt-10  hover:bg-gray-500 text-white font-bold py-2 px-4 rounded" @click="visible=true">Change Columns</button>
    <CoinModal :isVisible="visible"  @cancel="visible = false" @confirm="updateTable" :default_cols=columns></CoinModal>
    <CoinTable :cols=columns :coins=coins></CoinTable>
    
  </div>
</template>

<script>
import axios from 'axios'
import CoinTable from './components/Table.vue'
import CoinModal from './components/CustomModal.vue'

export default {
  name: 'App',
  components: {
   CoinTable,
   CoinModal
  },
  data() {
    return{  
    visible: false,
      coins: [],
      loading: true,
      errored: false,
      columns:['image','name','current_price','market_cap','price_change_percentage_24h']
    }
  },
  methods:{
    updateTable(cols){
      this.columns=cols;
      this.visible=false
    }
  },
  mounted() {
    axios
      .get('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
      .then(response => {
        this.coins = response.data
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
