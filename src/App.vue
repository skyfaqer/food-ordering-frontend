<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab-wrapper">
      <tab :tabs="tabs" :initial-index=0></tab>
    </div>
  </div>
</template>

<script>
  import VHeader from 'components/v-header/v-header'
  import Tab from 'components/tab/tab'
  import Goods from 'components/goods/goods'
  import Ratings from 'components/ratings/ratings'
  import Seller from 'components/seller/seller'
  import { getSeller } from 'api'
  import qs from 'query-string'

  export default {
    name: 'app',
    data() {
      return {
        seller: {
          id: qs.parse(location.search).id
        }
      }
    },
    computed: {
      tabs() {
        return [{
          label: 'Goods', component: Goods, data: { seller: this.seller }
        }, {
          label: 'Ratings', component: Ratings, data: { seller: this.seller }
        }, {
          label: 'Seller', component: Seller, data: { seller: this.seller }
        }]
      }
    },
    created() {
      this._getSeller()
    },
    methods: {
      _getSeller() {
        getSeller({ id: this.seller.id }).then((seller) => {
          this.seller = seller
        })
      }
    },
    components: {
      VHeader,
      Tab
    }
  }
</script>

<style lang="stylus">
  #app
    .tab-wrapper
      position: fixed
      top: 136px
      left: 0
      right: 0
      bottom: 0
</style>
