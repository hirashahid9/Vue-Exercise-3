<template>
<div class="hello">
    <br>
    <table class="rounded-t-lg m-5 w-5/6 mx-auto bg-gray-200 text-gray-800">
    <tr class="border-b-2 border-gray-300 text-center">
        <th v-for="col in cols" :key="col" @click="sort(col)" class="hover:bg-gray-500"> 
            {{col | upperCased }}
        </th>
    </tr>
    <tr class="bg-gray-100 border-b border-gray-200" v-for="coin in sortedCoins" :key="coin.id">
        <td v-for="col in cols" :key="col">
            <span v-if="col==='image'"><img :src="coin[col]" alt="coin img" style="height: 30px; width:30px"/> </span>
            <span v-else-if="col==='name'"> {{coin['name']}} ({{coin['symbol']}})  </span>
            <span v-else> {{coin[col]}}</span>
        </td>
    </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: 'CoinTable',
  props: {
    cols: [],  
    coins:[],
  },
  data() {
    return {
        currentSort:'',
        currentSortDir:'asc'      
        }
  },
  methods: {
      sort:function(s) {
        //if s == current sort, reverse
        if(s === this.currentSort) {
        this.currentSortDir = this.currentSortDir==='asc'?'desc':'asc';
        }
            this.currentSort = s;
    },
  },

  computed: {
      sortedCoins:function() {
      return this.coins.slice().sort((a,b) => {
        let modifier = 1;
        if(this.currentSortDir === 'desc') modifier = -1;
        if(a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
        if(a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
        return 0;
        });
    }
    },
  filters: {
    upperCased: function (data) {
      var upper = [];
      data.split("_").forEach((word) => {
        upper.push(word[0].toUpperCase()+word.slice(1));
      });
      return upper.join(" ");
    },
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
