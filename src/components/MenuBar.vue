<template>
    <div class="menu-bar">
      <transition name="slide-up">
        <div class="menu-wrapper" :class="{'hide-box-shadow': !ifShowBar||ifShowFontSize}" v-show="ifShowBar">
          <div class="icon-wrapper">
            <span class="icon-menu icon"></span>
          </div>
          <div class="icon-wrapper">
            <span class="icon-progress icon"></span>
          </div>
          <div class="icon-wrapper">
            <span class="icon-bright icon"></span>
          </div>
          <div class="icon-wrapper">
            <span class="icon-a icon" @click="ShowFontSizeBar">A</span>
          </div>
        </div>
      </transition>
      <transition name="slide-up">
        <div class="setting-wrapper" v-show="ifShowFontSize">
          <div class="setting-font-size">
            <div class="preview"
                 :style="{fontSize:fontSizeList[0].fontSize + 'px'}">A</div>
            <div class="select-wrapper">
              <div class="line"></div>
              <div class="point-wrapper"></div>
            </div>
            <div class="preview"
                 :style="{fontSize:fontSizeList[fontSizeList.length - 1].fontSize + 'px'}">A</div>
          </div>
        </div>
      </transition>
    </div>
</template>

<script>
export default {
  props: {
    ifShowBar: {
      type: Boolean,
      default: false
    },
    fontSizeList: {
      type: Array
    }
  },
  data () {
    return {
      ifShowFontSize: false
    }
  },
  methods: {
    ShowFontSizeBar () {
      this.ifShowFontSize = !this.ifShowFontSize
    },
    HideFontSizeBar () {
      this.ifShowFontSize = false
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/styles/global.scss';
.menu-bar {
  .menu-wrapper {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: px2rem(48);
    z-index: 101;
    display: flex;
    background: wheat;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .20);
    &.hide-box-shadow {
      box-shadow: none;
    }
    .icon-wrapper {
      flex: 1;
      @include center;
      .icon-progress,.icon-bright{
        font-size: px2rem(24);
      }
    }
  }
  .setting-wrapper {
    position:absolute;
    bottom: px2rem(48);
    left: 0;
    width: 100%;
    height: px2rem(60);
    z-index: 101;
    background: wheat;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .20);
    .setting-font-size {
      display: flex;
      height: 100%;
      .preview {
        flex: 0 0 px2rem(40);
        @include center;
      }
      .select-wrapper {
        flex: 1;
      }
    }
  }
}
</style>
