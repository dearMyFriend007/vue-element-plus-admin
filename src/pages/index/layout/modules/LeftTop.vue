<template>
  <div :class="classObj" class="app__wrap">
    <!-- Classic -->
    <div
      id="sidebar__wrap"
      class="sidebar__wrap"
      :class="{'sidebar__wrap--collapsed': collapsed}"
    >
      <logo
        v-if="showLogo && layout === 'Classic'"
        :collapsed="collapsed"
      />
      <sider :layout="layout" mode="vertical" />
    </div>
    <!-- Classic -->

    <!-- Top -->
    <div v-if="layout !== 'Classic'" class="sidebar__wrap--Top">
      <div>
        <logo
          v-if="showLogo"
          :collapsed="collapsed"
        />
      </div>
      <div v-if="layout === 'Top'" class="sidebar__item--Top">
        <sider :layout="layout" mode="horizontal" />
      </div>
      <div>
        <div v-if="showScreenfull || showUserInfo" class="navbar__wrap--right">
          <screenfull v-if="showScreenfull" class="hover-container screenfull-container" />
          <user-info v-if="showUserInfo" class="hover-container user-container" />
        </div>
      </div>
    </div>
    <!-- Top -->

    <div
      class="main__wrap"
      :class="{
        'main__wrap--collapsed': collapsed
      }"
    >
      <el-scrollbar
        class="main__wrap--content"
        :class="{
          'main__wrap--fixed--all': fixedHeader && showNavbar && showTags,
          'main__wrap--fixed--nav': fixedHeader && showNavbar && !showTags,
          'main__wrap--fixed--tags': fixedHeader && !showNavbar && showTags
        }"
      >
        <div
          class="header__wrap"
          :class="{
            'header__wrap--fixed': fixedHeader,
            'header__wrap--collapsed': fixedHeader && collapsed
          }"
        >
          <div
            v-if="showNavbar && layout !== 'Top'"
            class="navbar__wrap"
          >
            <hamburger
              v-if="showHamburger"
              id="hamburger-container"
              :collapsed="collapsed"
              class="hover-container"
              @toggleClick="setCollapsed"
            />
            <breadcrumb v-if="showBreadcrumb" id="breadcrumb-container" />
            <!-- <div v-if="showScreenfull || showUserInfo" class="navbar__wrap--right">
              <screenfull v-if="showScreenfull" class="hover-container screenfull-container" />
              <user-info v-if="showUserInfo" class="hover-container user-container" />
            </div> -->
          </div>
          <div
            v-if="showTags"
            id="tag-container"
            class="tags__wrap"
          >
            <tags-view />
          </div>
        </div>
        <app-main />
      </el-scrollbar>
    </div>

    <!-- setting -->
    <setting />
    <!-- setting -->

    <backtop v-if="showBackTop" />
  </div>
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue'
import { appStore } from '_@/store/modules/app'

import AppMain from '../components/AppMain.vue'
import TagsView from '_c/TagsView/index.vue'
import Logo from '_c/Logo/index.vue'
import Sider from '_c/Sider/index.vue'
import Hamburger from '_c/Hamburger/index.vue'
import Breadcrumb from '_c/Breadcrumb/index.vue'
import Screenfull from '_c/Screenfull/index.vue'
import UserInfo from '_c/UserInfo/index.vue'

import Setting from '_c/Setting/index.vue'
import Backtop from '_c/Backtop/index.vue'
export default defineComponent({
  name: 'LeftTop',
  components: {
    Sider,
    Hamburger,
    Breadcrumb,
    Screenfull,
    UserInfo,
    AppMain,
    TagsView,
    Logo,
    Setting,
    Backtop
  },
  setup() {
    const layout = computed(() => appStore.layout)
    const collapsed = computed(() => appStore.collapsed)
    const showLogo = computed(() => appStore.showLogo)
    const showTags = computed(() => appStore.showTags)
    const showBreadcrumb = computed(() => appStore.showBreadcrumb)
    const showHamburger = computed(() => appStore.showHamburger)
    const showScreenfull = computed(() => appStore.showScreenfull)
    const showUserInfo = computed(() => appStore.showUserInfo)
    const showNavbar = computed(() => appStore.showNavbar)
    // const fixedNavbar = computed(() => appStore.fixedNavbar)
    // const fixedTags = computed(() => appStore.fixedTags)
    const fixedHeader = computed(() => appStore.fixedHeader)
    const showBackTop = computed(() => appStore.showBackTop)

    const classObj = computed(() => {
      const obj = {}
      obj[`app__wrap--${layout.value}`] = true
      return obj
    })

    function setCollapsed(collapsed: boolean): void {
      appStore.SetCollapsed(collapsed)
    }

    return {
      classObj,
      layout,
      collapsed,
      showLogo,
      showTags,
      showBreadcrumb,
      showHamburger,
      showScreenfull,
      showUserInfo,
      showNavbar,
      fixedHeader,
      // fixedNavbar,
      // fixedTags,
      setCollapsed,
      showBackTop
    }
  }
})
</script>

<style lang="less" scoped>
@import  './style.less';
</style>
