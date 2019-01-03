<template>
    <transition name="slide-down">
      <div class="title-wrapper" v-show="ifShowBar">
        <div class="left">
          <span class="icon-back icon"></span>
        </div>
        <div class="right">
          <div class="icon-wrapper">
            <div class="icon-cart icon"></div>
          </div>
          <div class="icon-wrapper">
            <div class="icon-person icon"></div>
          </div>
          <div class="icon-wrapper">
            <div class="icon-more icon"></div>
          </div>
        </div>
      </div>
    </transition>
</template>

<script>
export default {
  props: {
    ifShowBar: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/styles/global.scss';
.title-wrapper {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: px2rem(48);
    z-index: 101;
    display: flex;
    background: wheat;
    box-shadow: 0 px2rem(8) px2rem(8) rgba(0, 0, 0, .20);
    .left {
      flex: 0 0 px2rem(60);
      @include center;
    }
    .right {
      flex: 1;
      display: flex;
      justify-content: flex-end;
      .icon-wrapper {
        flex:0 0 px2rem(40);
        @include center;
        .icon-cart {
          font-size: px2rem(22);
        }
      }
    }
  }
</style>
