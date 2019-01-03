<template>
  <div class="ebook">
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
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="toggleShowBar"></div>
        <div class="right" @click="nextPage"></div>
      </div>
    </div>
    <transition name="slide-up">
      <div class="menu-wrapper" v-show="ifShowBar">
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
          <span class="icon-a icon">A</span>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import Epub from 'epubjs'
const DOWNLOAD_URL = '/static/sfdl.epub'
export default {
  data () {
    return {
      ifShowBar: false
    }
  },
  methods: {
    toggleShowBar() {
      this.ifShowBar = !this.ifShowBar
    },
    prevPage() {
      if (this.rendition) {
        if (this.ifShowBar) {
          this.ifShowBar = false
        } else {
          this.rendition.prev()
        }
      }
    },
    nextPage() {
      if (this.rendition) {
        if (this.ifShowBar) {
          this.ifShowBar = false
        } else {
          this.rendition.next()
        }
      }
    },
    // 电子书的解析和渲染
    showEpub() {
      // 生成BOOK
      this.book = new Epub(DOWNLOAD_URL)
      // 生成Rendtion
      this.rendition = this.book.renderTo('read', {
        width: window.innerWidth,
        height: window.innerHeight
      })
      // 通过Renditon.display 渲染电子书
      this.rendition.display()
    }
  },
  mounted () {
    this.showEpub()
  }
}
</script>
<style lang='scss' scoped>
@import 'assets/styles/global.scss';
.ebook {
  position: relative;
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
  .read-wrapper {
    .mask {
      position: absolute;
      top: 0;
      left: 0;
      z-index: 100;
      display: flex;
      width: 100%;
      height: 100%;
      .left {
        flex: 0 0 px2rem(100);
      }
      .center {
        flex: 1
      }
      .right {
         flex: 0 0 px2rem(100);
      }
    }
  }
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
    .icon-wrapper {
      flex: 1;
      @include center;
      .icon-progress,.icon-bright{
        font-size: px2rem(24);
      }
    }
  }
}
</style>
