<template lang='pug'>
  div(v-if="isVisible")
    div.fixed.inset-0.z-50.flex.justify-center.items-center
      div.flex.flex-col.rounded-lg.shadow-lg.bg-white

        // header
        div.px-3.py-2.bg-gray-700.rounded-t
          div.flex.justify-between.items-start
            h3.text-2xl.pt-2.font-semibold.text-white Add/Remove Columns
            button.p-3.leading-none(@click="emitCancel")
              div.text-xl.font-semibold.h-6.w-6
                span.text-white x

        // body
        div.p-6
          <div class="grid grid-cols-2 gap-4 text-left">
          <div v-for="(opt,index) in options" :key="opt" class="leading-tight">
            <input type="checkbox" name="optCheck" :value="opt" v-model="selected_cols"/>
            <label class="pl-2 font-semibold">{{opt}}</label>
          </div>
          </div>

        // footer
        div.p-6.flex.justify-end.items-center
          <button class="bg-gray-500 hover:bg-gray-700 px-3 py-1 text-white rounded" @click="emitCancel"> Cancel</button>
          <button class="bg-blue-500 ml-3 hover:bg-blue-700 px-3 py-1 text-white rounded" @click="emitConfirm">Confirm</button>

    div.opacity-50.fixed.inset-0.z-40.bg-black
</template>

<script>
export default {
  name: 'CoinModal',
  props: {
    default_cols: [],
    isVisible: Boolean,
  },
  data() {
    return {
      selected_cols: this.default_cols,      
      
  options: ['id','symbol','name','image','current_price','market_cap','market_cap_rank','fully_diluted_valuation','total_volume',
            'high_24h','low_24h','price_change_24h','price_change_percentage_24h','market_cap_change_24h',
            'market_cap_change_percentage_24h','circulating_supply','total_supply','max_supply','ath','ath_change_percentage',
            'ath_date','atl','atl_change_percentage','atl_date','roi','last_updated']
    }
  },
  methods:{
     isPresent : function(value){
      for(var i=0; i < this.default_cols.length; i++){
        if( this.default_cols[i] == value){
          return true
        }
      }
      return false
    },
    emitCancel() {
      console.log("cancel called")
      this.$emit('cancel');
    },

    emitConfirm() {
      console.log("Confirm called")
      this.$emit('confirm',this.selected_cols)
    }

  }
    /*roi:{
          times: 0, 
          currency: '', 
          percentage: 0 
        },*/
}
</script>
