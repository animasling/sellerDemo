<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" width="64" height="64"/>
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%"/>
    </div>
    <transition name="fade">
    <div v-show="detailShow" class="detail">
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <h1 class="name">{{seller.name}}</h1>
          <div class="star-wrapper">
            <star :size="48" :score="seller.score"></star>
          </div>
          <div class="title">
            <div class="line"></div>
            <div class="text">优惠信息</div>
            <div class="line"></div>
          </div>
          <ul v-if="seller.supports" class="supports">
            <li class="support-item" v-for="(item,index) in seller.supports">
              <span class="icon" :class="classMap[seller.supports[index].type]"></span>
              <span class="text">{{seller.supports[index].description}}</span>
            </li>
          </ul>
          <div class="title">
            <div class="line"></div>
            <div class="text">商家公告</div>
            <div class="line"></div>
          </div>
          <div class="bulletin">
            <p class="content">{{seller.bulletin}}</p>
          </div>
        </div>
      </div>
      <div class="detail-close" @click="hideDetail">
        <i class="icon-close"></i>
      </div>
    </div>
    </transition>
  </div>
</template>
<script type="text/ecmascript-6">
  import star from '@/components/star/star';
  export default{
    props: {
      seller: Object
    },
    data() {
      return {
        detailShow: false
      };
    },
    methods: {
      showDetail() {
        this.detailShow = true;
      },
      hideDetail() {
        this.detailShow = false;
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components: {
      star
    }
  };
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../../common/sass/mixin.scss";

  .header {
    position: relative;
    color: #fff;
    background-color: rgba(7, 17, 27, .5);
    overflow: hidden;
    .content-wrapper {
      position: relative;
      padding: 24px 12px 18px 24px;
      font-size: 0;
      .avatar {
        display: inline-block;
        vertical-align: top;
        img {
          border-radius: 2px;
        }
      }
      .content {
        display: inline-block;
        margin-left: 16px;
        .title {
          margin: 1px 0 4px 0;
          .brand {
            @include bg-image('brand');
            display: inline-block;
            width: 30px;
            height: 18px;
            vertical-align: top;
            background: {
              size: 30px 18px;
              repeat: no-repeat;
            }
          }
          .name {
            margin-left: 6px;
            font: {
              size: 16px;
              weight: bold;
            }
            line-height: 18px;
          }
        }
        .description {
          margin-bottom: 10px;
          line-height: 12px;
          font-size: 12px;
        }
        .support {
          .icon {
            display: inline-block;
            margin-right: 4px;
            width: 12px;
            height: 12px;
            vertical-align: top;
            background: {
              size: 12px 12px;
              repeat: no-repeat;
            }
            &.decrease {
              @include bg-image('decrease_1');
            }
            &.discount {
              @include bg-image('discount_1');
            }
            &.guarantee {
              @include bg-image('guarantee_1');
            }
            &.invoice {
              @include bg-image('invoice_1');
            }
            &.special {
              @include bg-image('special_1');
            }
          }
          .text {
            line-height: 12px;
            font-size: 10px;
          }
        }
      }
      .support-count {
        position: absolute;
        padding: 0 8px;
        right: 12px;
        bottom: 14px;
        height: 24px;
        line-height: 24px;
        border-radius: 14px;
        background-color: rgba(0, 0, 0, .2);
        text-align: center;
        .count {
          font-size: 10px;
        }
        .icon-keyboard_arrow_right {
          margin-left: 2px;
          line-height: 24px;
          font-size: 10px;
        }
      }
    }
    .bulletin-wrapper {
      position: relative;
      padding: 0 22px 0 12px;
      height: 28px;
      line-height: 28px;
      vertical-align: middle;
      background-color: rgba(7, 17, 27, .2);
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      .bulletin-title {
        @include bg-image("bulletin");
        display: inline-block;
        margin-top: 9px;
        width: 22px;
        height: 12px;
        vertical-align: top;
        background: {
          size: 22px 12px;
          repeat: no-repeat;
        }
      }
      .bulletin-text {
        margin: 0 4px;
        font-size: 10px;
      }
      .icon-keyboard_arrow_right {
        position: absolute;
        top: 9px;
        right: 12px;
        font-size: 10px;
      }
    }
    .background {
      position: absolute;
      top: 0;
      left: 0;
      z-index: -1;
      width: 100%;
      height: 100%;
      filter: blur(10px);
    }
    .detail {
      position: fixed;
      top: 0;
      left: 0;
      z-index: 100;
      width: 100%;
      height: 100%;
      overflow: auto;
      opacity:1;
      background:rgba(7,17,27,.8);
      backdrop-filter:blur(10px);/*IOS才支持背景全模糊*/
      &.fade-enter-active,&.fade-leave-active{
        transition:all .5s;
      }
      &.fade-enter,&.fade-leave-active{
       opacity:0;
       background:rgba(7,17,27,0);
      }
      .detail-wrapper {
        min-height: 100%;
        width: 100%;
        .detail-main {
          margin-top: 64px;
          padding-bottom: 64px;
          .name {
            line-height: 16px;
            text-align: center;
            font: {
              size: 16px;
              weight: 700;
            }
          }
          .star-wrapper {
            margin-top: 18px;
            padding: 2px;
            text-align: center;
          }
          .title {
            display: flex;
            margin: 28px auto 24px auto;
            width: 80%;
            .line {
              flex: 1;
              position: relative;;
              top: -6px;
              border-bottom: 1px solid rgba(255, 255, 255, .2);
            }
            .text {
              padding: 0 12px;
              font: {
                size: 14px;
                weight: 700;
              }
            }
          }
          .supports {
            margin: 0 auto;
            width: 80%;
            .support-item {
              margin-bottom: 12px;
              padding: 0 12px;
              font-size: 0;
              &.last-child {
                margin-bottom: 0;
              }
              .icon {
                display: inline-block;
                margin-right: 6px;
                width: 16px;
                height: 16px;
                vertical-align: top;
                background: {
                  size: 16px 16px;
                  repeat: no-repeat;
                }
                &.decrease {
                  @include bg-image('decrease_2');
                }
                &.discount {
                  @include bg-image('discount_2');
                }
                &.guarantee {
                  @include bg-image('guarantee_2');
                }
                &.invoice {
                  @include bg-image('invoice_2');
                }
                &.special {
                  @include bg-image('special_2');
                }
              }
              .text {
                line-height: 16px;
                font-size: 12px;
              }
            }
          }
          .bulletin {
            margin: 0 auto;
            width: 80%;
            .content {
              padding: 0 12px;
              line-height: 24px;
              font-size: 12px;
            }
          }
        }
      }
      .detail-close {
        position: relative;
        margin: -64px auto 0 auto;
        width: 32px;
        height: 32px;
        clear: both;
        font-size: 32px;
      }
    }
  }
</style>
