<template>
  <div class="page-header-index-wide">
    <a-card :bordered="false" :bodyStyle="{ padding: '16px 0', height: '100%' }" :style="{ height: '100%' }">
      <div class="account-settings-info-main" :class="{ mobile: isMobile }">
        <div class="account-settings-info-left">
          <a-menu :mode="isMobile ? 'horizontal' : 'inline'" :style="{ border: '0', width: isMobile ? '560px' : 'auto' }" :selectedKeys="selectedKeys" type="inner">
            <a-menu-item v-for="r in sameLevelRouters" :key="r.path">
              <router-link :to="{ name: r.name }">
                {{ $t(r.meta.title) }}
              </router-link>
            </a-menu-item>
          </a-menu>
        </div>
        <div class="account-settings-info-right">
          <route-view></route-view>
        </div>
      </div>
    </a-card>
  </div>
</template>

<script>
import { RouteView } from '@/layouts'
import { baseMixin } from '@/store/app-mixin'

export default {
  components: {
    RouteView
  },
  mixins: [baseMixin],
  data () {
    return {
      mode: 'inline', // horizontal  inline
      selectedKeys: [],
      sameLevelRouters: []
    }
  },
  mounted () {
    this.updateMenu()
  },
  methods: {
    updateMenu () {
      const routes = this.$route.matched.concat()
      this.selectedKeys = [routes.pop().path]
      const routers = [...this.$store.getters.addRouters]
      const currPath = this.$route.fullPath
      this.sameLevelRouters = [...this.searchSameLevelRouters(routers, currPath)]
    },
    searchSameLevelRouters (routers, currPath) {
      let sameLevelRouters = []
      for (let i = 0; i < routers.length; i++) {
        const r = routers[i]
        if (r.path === currPath && sameLevelRouters.length === 0) {
          console.log('matched--------', r.path, currPath)
          sameLevelRouters = routers
        } else if (r.children && sameLevelRouters.length === 0) {
          sameLevelRouters = this.searchSameLevelRouters(r.children, currPath)
        }
        if (sameLevelRouters.length > 0) break
      }
      return sameLevelRouters
    }
  },
  watch: {
    $route (val) {
      this.updateMenu()
    }
  }
}
</script>

<style lang="less" scoped>
.account-settings-info-main {
  width: 100%;
  display: flex;
  height: 100%;
  overflow: auto;

  &.mobile {
    display: block;

    .account-settings-info-left {
      border-right: unset;
      border-bottom: 1px solid #e8e8e8;
      width: 100%;
      height: 50px;
      overflow-x: auto;
      overflow-y: scroll;
    }
    .account-settings-info-right {
      padding: 20px 40px;
    }
  }

  .account-settings-info-left {
    border-right: 1px solid #e8e8e8;
    width: 135px;
  }

  .account-settings-info-right {
    background: #f0f2f5;
    flex: 1 1;
    padding: 0;

    .account-settings-info-title {
      color: rgba(0, 0, 0, 0.85);
      font-size: 20px;
      font-weight: 500;
      line-height: 28px;
      margin-bottom: 12px;
    }
    .account-settings-info-view {
      padding-top: 12px;
    }
  }
}
</style>
