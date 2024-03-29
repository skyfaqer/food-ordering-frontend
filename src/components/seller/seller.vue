<template>
  <cube-scroll class="seller" :options="sellerScrollOptions">
    <div class="seller-content">
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc border-bottom-1px">
          <star :size="36" :score="seller.score"></star>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">Sales: {{seller.sellCount}}</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2>Starting total</h2>
            <div class="content">
              <span class="stress">￥{{seller.minPrice}}</span>
            </div>
          </li>
          <li class="block">
            <h2>Delivery price</h2>
            <div class="content">
              <span class="stress">￥{{seller.deliveryPrice}}</span>
            </div>
          </li>
          <li class="block">
            <h2>Average delivery time</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}</span>min
            </div>
          </li>
        </ul>
        <div class="favorite" @click="toggleFavorite">
          <span class="icon-favorite" :class="{'active':favorite} "></span>
          <span class="text">{{favoriteText}}</span>
        </div>
      </div>
      <split></split>
      <div class="bulletin">
        <h1 class="title">Bulletin</h1>
        <div class="content-wrapper border-bottom-1px">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul v-if="seller.supports" class="supports">
          <li
            class="support-item border-bottom-1px"
            v-for="(item,index) in seller.supports"
            :key="index">
            <support-ico :size=4 :type="seller.supports[index].type"></support-ico>
            <span class="text">{{seller.supports[index].description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics">
        <h1 class="title">Pictures</h1>
        <cube-scroll class="pic-wrapper" :options="picScrollOptions">
          <ul class="pic-list">
            <li class="pic-item"
                v-for="(pic,index) in seller.pics"
                :key="index">
              <img :src="pic" width="120" height="90">
            </li>
          </ul>
        </cube-scroll>
      </div>
      <split></split>
      <div class="info">
        <h1 class="title border-bottom-1px">Seller info</h1>
        <ul>
          <li
            class="info-item border-bottom-1px"
            v-for="(info,index) in seller.infos"
            :key="index">
            {{info}}
          </li>
        </ul>
      </div>
    </div>
  </cube-scroll>
</template>

<script>
  import Star from 'components/star/star'
  import Split from 'components/split/split'
  import SupportIco from 'components/support-ico/support-ico'
  import { saveToLocal, loadFromLocal } from 'common/js/storage'

  const KEY_FAVORITE = 'favorite'

  export default {
    props: {
      data: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    data() {
      return {
        favorite: false,
        sellerScrollOptions: {
          click: false,
          directionLockThreshold: 0
        },
        picScrollOptions: {
          scrollX: true,
          stopPropagation: true,
          directionLockThreshold: 0
        }
      }
    },
    computed: {
      seller() {
        return this.data.seller || {}
      },
      favoriteText() {
        return this.favorite ? 'Liked' : 'Like'
      }
    },
    created() {
      this.favorite = loadFromLocal(this.seller.id, KEY_FAVORITE, false)
    },
    methods: {
      toggleFavorite() {
        this.favorite = !this.favorite
        saveToLocal(this.seller.id, KEY_FAVORITE, this.favorite)
      }
    },
    components: {
      SupportIco,
      Star,
      Split
    }
  }
</script>

<style lang="stylus" scoped>
  @import "~common/stylus/variable"
  @import "~common/stylus/mixin"

  .seller
    height: 100%
    text-align: left
    .overview
      position: relative
      padding: 18px
      .title
        margin-bottom: 8px
        line-height: 14px
        font-size: $fontsize-medium
        color: $color-dark-grey
      .desc
        display: flex
        align-items: center
        padding-bottom: 18px
        .star
          margin-right: 8px
        .text
          margin-right: 12px
          line-height: 18px
          font-size: $fontsize-small-s
          color: $color-grey
      .remark
        display: flex
        padding-top: 18px
        .block
          flex: 1
          text-align: center
          border-right: 1px solid $color-col-line
          &:last-child
            border: none
          h2
            margin-bottom: 4px
            line-height: 10px
            font-size: $fontsize-small-s
            color: $color-light-grey
          .content
            line-height: 24px
            font-size: $fontsize-small-s
            color: $color-dark-grey
            .stress
              font-size: $fontsize-large-xxx
      .favorite
        position: absolute
        width: 50px
        right: 11px
        top: 18px
        text-align: center
        .icon-favorite
          display: block
          margin-bottom: 4px
          line-height: 24px
          font-size: $fontsize-large-xxx
          color: $color-light-grey-s
          &.active
            color: $color-red
        .text
          line-height: 10px
          font-size: $fontsize-small-s
          color: $color-grey
    .bulletin
      padding: 18px 18px 0 18px
      white-space: normal
      .title
        margin-bottom: 8px
        line-height: 14px
        color: $color-dark-grey
        font-size: $fontsize-medium
      .content-wrapper
        padding: 0 12px 16px 12px
        .content
          line-height: 24px
          font-size: $fontsize-small
          color: $color-red
      .supports
        .support-item
          display: flex
          align-items: center
          padding: 16px 12px
          &:last-child
            border-none()
        .support-ico
          margin-right: 6px
        .text
          line-height: 16px
          font-size: $fontsize-small
          color: $color-dark-grey
    .pics
      padding: 18px
      .title
        margin-bottom: 12px
        line-height: 14px
        color: $color-dark-grey
        font-size: $fontsize-medium
      .pic-wrapper
        display: flex
        align-items: center
        .pic-list
          .pic-item
            display: inline-block
            margin-right: 6px
            width: 120px
            height: 90px
            &:last-child
              margin: 0
    .info
      padding: 18px 18px 0 18px
      color: $color-dark-grey
      .title
        padding-bottom: 12px
        line-height: 14px
        font-size: $fontsize-medium
      .info-item
        padding: 16px 12px
        line-height: 16px
        font-size: $fontsize-small
        white-space: normal
        &:last-child
          border-none()
</style>
