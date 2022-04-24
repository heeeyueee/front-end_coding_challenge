<template>
  <div class="header">
    <div class="title" >
      Where in the world?
    </div>
    <div class="go-github" @click="goGitHub">
      <i class="icon el-icon-s-promotion"></i> GitHub
    </div>
  </div>
</template>

<script lang="ts" setup>


// eslint-disable-next-line no-unused-vars


// eslint-disable-next-line no-unused-vars
const goGitHub = () => {
  window.open('https://github.com/heeeyueee/front-end_coding_challenge')
}
</script>

<style scoped lang="stylus">

.header {
  width 90%
  height 100%
  background #fff
  display flex
  justify-content space-between
  align-items center
  padding 20px 20px
  margin: 0 auto
  box-sizing border-box
  font-weight bold

  .title {
    font-size 20px
    cursor pointer
  }

  .go-github {
    cursor pointer
    font-size 16px

    .icon {
      font-size 20px
    }
  }
}
</style>
