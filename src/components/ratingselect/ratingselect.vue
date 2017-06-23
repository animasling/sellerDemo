<template>
<div class="ratingselect">
  <div class="rating-type border-1px">
    <span @click="select(2,$event)" class="block positive" :class="{'active':selectType===2}">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
    <span @click="select(0,$event)" class="block positive" :class="{'active':selectType===0}">{{desc.positive}}<span class="count">{{positives.length}}</span></span>
    <span @click="select(1,$event)" class="block negative" :class="{'active':selectType===1}">{{desc.negative}}<span class="count">{{negative.length}}</span></span>
  </div>
  <div class="switch" :class="{'on':onlyContent}" @click="toggleContent">
    <span class="icon-check_circle"></span>
    <span class="text">只看内容的评价</span>
  </div>
</div>
</template>
<script type="text/ecmascript-6">
  const POSITIVE = 0;
  const NEGATIVE = 1;
  const ALL = 2;
  export default {
    props: {
      ratings: {
        type: Array,
        default() {
          return [];
        }
      },
      selectType: {
        type: Number,
        default: ALL
      },
      onlyContent: {
        type: Boolean,
        default: false
      },
      desc: {
        type: Object,
        default() {
          return {
            all: '全部',
            positive: '满意',
            negative: '不满意'
          };
        }
      }
    },
    computed: {
      positives() {
        return this.ratings.filter((rating) => {
          return rating.rateType === POSITIVE;
        });
      },
      negative() {
        return this.ratings.filter((rating) => {
          return rating.rateType === NEGATIVE;
        });
      }
    },
    methods: {
      select(type, event) {
        if (!event._constructed) {
          return;
        }
        this.$emit('select', type);
      },
      toggleContent(event) {
        if (!event._constructed) {
          return;
        }
        this.$emit('toggle');
      }
    }
  };
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/sass/mixin";
.ratingselect{
  .rating-type{
    margin:0 18px;
    padding:18px 0;
    font-size:0;
    @include border-1px(rgba(7,17,27,.1));
    .block{
      display: inline-block;
      margin-right:8px;
      padding:8px 12px;
      font-size:12px;
      border-radius:2px;
      color:rgb(77,85,93);
      &.active{
        color:#fff;
      }
      &.positive{
        background-color:rgba(0,160,220,.2);
        &.active{
          background-color:rgb(0,160,220);
        }
      }
      &.negative{
        background-color:rgba(77,85,93,.2);
        &.active{
          background-color:rgb(77,85,93);
        }
      }
      .count{
        margin-left:2px;
        font-size:8px;
        line-height:16px;
        transform:scale(.666);
      }
    }
  }
  .switch{
    padding:12px 18px;
    line-height:24px;
    font-size:0;
    border-bottom:1px solid rgba(7,17,27,.1);
    color:rgb(147,153,159);
    &.on{
      .icon-check_circle{
        color:#00c850;
      }
    }
    .icon-check_circle{
      display: inline-block;
      margin-right:4px;
      font-size:24px;
      vertical-align: top;
    }
    .text{
      font-size:12px;
    }
  }
}
</style>
