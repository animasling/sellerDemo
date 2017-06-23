<template>
  <transition name="move">
    <div v-show="showFlag" class="food" ref="food">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
          <div class="back" @click="hide">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好频率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now">￥{{food.price}}</span><span v-show="food.oldPrice" class="old">{{food.oldPrice}}</span>
          </div>
          <div class="cartcontrol-wrapper">
            <cartcontrol :food="food" @add="addFood"></cartcontrol>
          </div>
          <transition name="fade">
            <div class="buy" v-show="!food.count || food.count===0" @click.stop.prevent="addFirst">加入购物车</div>
          </transition>
        </div>
        <split v-show="food.info"></split>
        <div class="info" v-show="food.info">
          <div class="title">商品信息</div>
          <div class="text">{{food.info}}</div>
        </div>
        <split></split>
        <div class="ratings">
          <div class="title">商品评价</div>
          <ratingselect :selectType="selectType" :onlyContent="onlyContent" :desc="desc" :ratings="food.ratings"
                        @select="changeRatingType" @toggle="ifOnlyContent"></ratingselect>
          <div class="rating-wrapper">
            <ul v-show="food.ratings && food.ratings.length">
              <li v-show="needShow(rating.rateType,rating.text)" v-for="rating in food.ratings" class="rating-item border-1px">
                <div class="user">
                  <span class="name">{{rating.username}}</span>
                  <img class="avatar" width="12" height="12" :src="rating.avatar">
                </div>
                <div class="time">{{rating.rateTime | formatDate}}</div>
                <div class="text">
                  <span :class="[rating.rateType===0?'icon-thumb_up':'icon-thumb_down']"></span>
                  {{rating.text}}
                </div>
              </li>
            </ul>
            <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import Vue from 'vue';
  import {formatDate} from '@/common/js/date.js';
  import cartcontrol from '@/components/cartcontrol/cartcontrol';
  import split from '@/components/split/split';
  import ratingselect from '@/components/ratingselect/ratingselect';

//  const POSITIVE = 0;
//  const NEGATIVE = 1;
  const ALL = 2;

  export default {
    props: {
      food: Object
    },
    data() {
      return {
        showFlag: false,
        selectType: ALL,
        onlyContent: true,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      };
    },
    methods: {
      show() {
        this.showFlag = true;
        this.selectType = ALL;
        this.onlyContent = true;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      },
      hide() {
        this.showFlag = false;
      },
      addFirst(event) {
        if (!event._constructed) {
          return;
        }
        this.$emit('add', event.target);
        Vue.set(this.food, 'count', 1);
      },
      addFood(target) {
        this.$emit('add', target);
      },
      needShow(type, text) {
        if (this.onlyContent && !text) {
            return false;
        }
        if (this.selectType === ALL) {
          return true;
        } else {
          return type === this.selectType;
        }
      },
      changeRatingType(type) {
        this.selectType = type;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      },
      ifOnlyContent() {
        this.onlyContent = !this.onlyContent;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      }
    },
    filters: {
      formatDate(time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh:mm');
      }
    },
    components: {
      cartcontrol, split, ratingselect
    }
  };
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/sass/mixin";
  .food {
    position: fixed;
    left: 0;
    top: 0;
    bottom: 48px;
    z-index: 30;
    width: 100%;
    background-color: #fff;
    transform: translate3d(0, 0, 0);
    &.move-enter-active, &.move-leave-active {
      transition: all .2s linear;
    }
    &.move-enter, &.move-leave-active {
      transform: translate3d(100%, 0, 0);
    }
    .image-header {
      position: relative;
      padding-top: 100%;
      width: 100%;
      height: 0;
      img {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
      }
      .back {
        position: absolute;
        top: 10px;
        left: 0;
        .icon-arrow_lift {
          display: block;
          padding: 18px;
          font-size: 20px;
          color: #fff;
        }
      }
    }
    .content {
      position: relative;
      padding: 18px;
      .title {
        line-height: 14px;
        font: {
          size: 14px;
          weight: bold;
        }
        color: rgb(7, 17, 27);
      }
      .detail {
        margin: 8px 0 18px 0;
        height: 10px;
        line-height: 10px;
        font-size: 0;
      }
      .sell-count, .rating {
        font-size: 10px;
        color: rgb(147, 153, 159);
      }
      .sell-count {
        margin-right: 12px;
      }
      .price {
        font-weight: 700;
        line-height: 24px;
        .now {
          margin-right: 8px;
          font-size: 14px;
          color: rgb(240, 20, 20)
        }
        .old {
          font-size: 10px;
          color: rgb(147, 153, 159);
          text-decoration: line-through;
        }
      }
      .cartcontrol-wrapper {
        position: absolute;
        right: 18px;
        bottom: 12px;
      }
      .buy {
        position: absolute;
        right: 18px;
        bottom: 18px;
        z-index: 10;
        padding: 0 12px;
        height: 24px;
        line-height: 24px;
        box-sizing: border-box;
        font: {
          size: 10px;
        }
        border-radius: 12px;
        color: #fff;
        background-color: rgb(0, 160, 220);
        opacity: 1;
        &.fade-enter-active, &.fade-leave-active {
          transition: all .2s;
        }
        &.fade-enter, &.fade-leave {
          opacity: 0;
        }
      }
    }
    .info {
      padding: 18px;
      .title {
        margin-bottom: 6px;
        line-height: 14px;
        color: rgb(7, 17, 27);
        font: {
          size: 14px;
          weight: bold;
        }
      }
      .text {
        padding: 0 8px;
        line-height: 24px;
        font-size: 12px;
        color: rgb(77, 85, 93);
      }
    }
    .ratings{
      padding:18px 0;
      .title{
        margin-left: 18px;
        line-height: 14px;
        color: rgb(7, 17, 27);
        font: {
          size: 14px;
          weight: bold;
        }
      }
      .rating-wrapper{
        padding:0 18px;
        .rating-item{
          position:relative;
          padding:16px 0;
          @include  border-1px(rgba(7,17,27,.1));
          .user{
            position:absolute;
            right:0;
            top:16px;
            font-size:0;
            line-height:12px;
            .name{
              display: inline-block;
              margin-right:6px;
              vertical-align: top;
              font-size:10px;
              color:rgb(147,153,159);
            }
            .avatar{
              border-radius:50%;
            }
          }
          .time{
            margin-bottom:6px;
            font-size:10px;
            line-height:12px;
            color:rgb(147,153,159);
          }
          .text{
            line-height:16px;
            font-size:12px;
            color:rgb(7,17,27);
          }
          .icon-thumb_up,.icon-thumb_down{
            margin-right:4px;
            font-size:12px;
            line-height:12px;
          }
          .icon-thumb_up{
            color:rgb(0,160,220);
          }
          .icon-thumb_down{
            color:rgb(147,153,159);
          }
        }
        .no-rating{
          padding:16px 0;
          font-size:12px;
          color:rgb(147,153,159);
        }
      }
    }
  }
</style>
