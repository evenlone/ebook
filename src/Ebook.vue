<template>
  <div class="ebook">
    <title-bar :ifShowBar="ifShowBar"></title-bar>
    <div class="read-wrapper">
      <div id="read"></div>
      <div class="mask">
        <div class="left" @click="prevPage"></div>
        <div class="center" @click="toggleShowBar"></div>
        <div class="right" @click="nextPage"></div>
      </div>
    </div>
    <menu-bar :ifShowBar="ifShowBar"
              :fontSizeList="fontSizeList"
              :defaultFontSize="defaultFontSize"
              ref="menuBar"
              ></menu-bar>
  </div>
</template>

<script>
import TitleBar from '@/components/TitleBar'
import MenuBar from '@/components/MenuBar'
import Epub from 'epubjs'
const DOWNLOAD_URL = '/static/sfdl.epub'
export default {
  components: {
    TitleBar,
    MenuBar
  },
  data () {
    return {
      ifShowBar: false,
      fontSizeList: [
        {fontSize: 12},
        {fontSize: 14},
        {fontSize: 16},
        {fontSize: 18},
        {fontSize: 20},
        {fontSize: 22},
        {fontSize: 24}
      ],
      defaultFontSize: 16
    }
  },
  methods: {
    toggleShowBar() {
      this.ifShowBar = !this.ifShowBar
      if (!this.ifShowBar) {
        this.$refs.menuBar.HideFontSizeBar()
      }
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
          this.$refs.menuBar.ifShowFontSize = false
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
        flex: 1;
      }
      .right {
         flex: 0 0 px2rem(100);
      }
    }
  }
}
</style>
