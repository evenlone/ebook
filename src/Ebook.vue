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
              @setFontSize="setFontSize"
              :themeList="themeList"
              :defaultTheme="defaultTheme"
              @setTheme="setTheme"
              :bookAvailable="bookAvailable"
              @onProgressChange="onProgressChange"
              :navigation="navigation"
              @jumpTo="jumpTo"
              ref="menuBar"></menu-bar>
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
      defaultFontSize: 16,
      themeList: [
        {
          name: 'default',
          style: {
            body: {
              'color': '#000', 'background': '#fff'
            }
          }
        },
        {
          name: 'eye',
          style: {
            body: {
              'color': '#222', 'background': '#ceeaba'
            }
          }
        },
        {
          name: 'night',
          style: {
            body: {
              'color': '#fff', 'background': '#222'
            }
          }
        },
        {
          name: 'gold',
          style: {
            body: {
              'color': '#000', 'background': 'rgb(241,236,226)'
            }
          }
        }
      ],
      defaultTheme: 0,
      bookAvailable: false
    }
  },
  methods: {
    // 章节导航
    jumpTo(href) {
      this.rendition.display(href)
      this.hideTitleAndMenu()
    },
    hideTitleAndMenu() {
      this.ifShowBar = false
      this.$refs.menuBar.hideSetting()
      this.$refs.menuBar.hideContent()
    },
    // 改变进度条
    onProgressChange(progress) {
      const percentage = progress / 100
      const location = percentage > 0 ? this.locations.cfiFromPercentage(percentage) : 0
      this.rendition.display(location)
    },
    registerTheme() {
      this.themeList.forEach(theme => {
        this.themes.register(theme.name, theme.style)
      })
    },
    setTheme(index) {
      this.themes.select(this.themeList[index].name)
      this.defaultTheme = index
    },
    setFontSize (fontSize) {
      this.defaultFontSize = fontSize
      if (this.themes) {
        this.themes.fontSize(fontSize + 'px')
      }
    },
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
          this.$refs.menuBar.HideFontSizeBar()
        } else {
          this.rendition.prev()
        }
      }
    },
    nextPage() {
      if (this.rendition) {
        if (this.ifShowBar) {
          this.ifShowBar = false
          this.$refs.menuBar.HideFontSizeBar()
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
      // themes 对象
      this.themes = this.rendition.themes
      // 默认字体
      this.setFontSize(this.defaultFontSize)
      // 注册 主题
      this.registerTheme()
      this.setTheme(this.defaultTheme)
      this.book.ready.then(() => {
        this.navigation = this.book.navigation
        return this.book.locations.generate()
      }).then(result => {
        this.locations = this.book.locations
        this.bookAvailable = true
        // this.onProgressChange(50)
      })
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
