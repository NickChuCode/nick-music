<template>
    <div class="tab">
        <router-link class="tab-item" tag="div" to="/recommend">
          <i class="icon-player"></i>
          <span class="tab-link">推荐</span>
        </router-link>
        <router-link class="tab-item" tag="div" to="/singer">
          <i class="icon-singer"></i>
          <span class="tab-link">歌手</span>
        </router-link>
        <router-link class="tab-item" tag="div" to="/rank">
          <i class="icon-diamond"></i>
          <span class="tab-link">排行</span>
        </router-link>
        <router-link class="tab-item" tag="div" to="/search">
          <i class="icon-like"></i>
          <span class="tab-link">搜索</span>
        </router-link>
    </div>
</template>
<script>
export default {}
</script>
<style lang="stylus" scoped>
  @import '~common/stylus/variable'
  .tab
      display flex
      height 100px
      line-height 44px
      font-size $font-size-medium
      .tab-item
          flex 1
          text-align center
          i
            display block
            font-size 31px
          .tab-link
            display block
            padding-bottom 5px
            color $color-text-l
          &.router-link-active
            .tab-link
              color $color-theme
              border-bottom 2px solid $color-theme
</style>
