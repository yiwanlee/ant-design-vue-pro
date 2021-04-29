<template>
  <a-config-provider :locale="locale">
    <div id="app" class="app app1">
      <router-view class="scrollbar"/>
    </div>
  </a-config-provider>
</template>

<script>
import { domTitle, setDocumentTitle } from '@/utils/domUtil'
import { i18nRender } from '@/locales'

export default {
  data () {
    return {
    }
  },
  computed: {
    locale () {
      // 只是为了切换语言时，更新标题
      const { title } = this.$route.meta
      title && (setDocumentTitle(`${i18nRender(title)} - ${domTitle}`))

      return this.$i18n.getLocaleMessage(this.$store.getters.lang).antLocale
    }
  }
}
</script>
<style>
  .app {
    overflow: auto;
    border  : none;
  }
  .scrollbar {
    margin: 0 auto;
  }
  .app1::-webkit-scrollbar {
    /*滚动条整体样式*/
    width : 8px;  /*高宽分别对应横竖滚动条的尺寸*/
  }
  .app1::-webkit-scrollbar-thumb {
    /*滚动条里面小方块*/
    border-radius: 6px;
    background   : #aaa;
  }
  .app1::-webkit-scrollbar-track {
    /*滚动条里面轨道*/
    border-radius: 8px;
    background   : #FFFFFF;
  }
  /*临时修改设置抽屉导致的横向滚动条*/
  body{
    overflow-x: hidden;
  }
</style>
