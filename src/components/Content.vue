<template>
  <transition name="slide-right">
    <div class="content">
      <div class="content-wrapper">
        <div class="content-item">
          <span class="text"></span>
        </div>
      </div>
      <div class="empty">加载中...</div>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    ifShowContent: Boolean,
    navigation: Object
  }
}
</script>
<style scoped>
</style>
