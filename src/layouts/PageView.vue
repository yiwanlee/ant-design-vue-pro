<template>
  <page-header-wrapper :title="false">
    <!--
      这里增加了 multiTab 的判断，当开启了 multiTab 时
      应当全部组件皆缓存，否则会导致切换页面后页面还原成原始状态
    -->
    <h1>{{ multiTab }}</h1>
    <keep-alive v-if="multiTab">
      <router-view ref="content" />
    </keep-alive>
    <router-view ref="content" v-else />
  </page-header-wrapper>
</template>

<script>
import store from '@/store'

export default {
  name: 'PageView',
  data() {
    return {
      multiTab: store.getters.multiTab
    }
  },
  mounted() {
    console.log(store.getters.multiTab)
  }
}
</script>
