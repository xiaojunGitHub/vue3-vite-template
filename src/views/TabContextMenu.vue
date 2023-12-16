<template>
  <ul
    v-show="menuState.visible"
    class="context-menu"
    :style="{ top: menuState.top, left: menuState.left }"
  >
    <li class="menu-item" @click="closeAllTabs">关闭所有</li>
    <li class="menu-item" @click="closeLeftTabs">关闭左侧</li>
    <li class="menu-item" @click="closeRightTabs">关闭右侧</li>
    <li class="menu-item" @click="closeOtherTabs">关闭其他</li>
  </ul>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import store from '@/store'
import { useRoute, useRouter } from 'vue-router'
// import { MenuStateInterface } from './tabs-view.vue'
import { RouteItem } from '@/store/modules/tabs-view/state'
export default defineComponent({
  name: 'TabContextMenu'
})
</script>

<script setup lang="ts">
const route = useRoute()
const router = useRouter()

let a = 'c'

const props = defineProps<{
  menuState: MenuStateInterface
}>()

// 获取右键的tab
const getTargetTab = () => {
  return props.menuState.routeInfo as RouteItem
}
// 检查是否右键当前激活页 或 当前页是否还存在
const checkCurrentTab = () => {
  console.log(route)
  if (store.state.tabsView.tabsList.findIndex((tab) => tab.fullPath === route.fullPath) !== -1)
    return
  if (route.fullPath !== getTargetTab().fullPath) {
    router.push({
      name: getTargetTab().name
    })
  }
}

// 关闭所有
const closeAllTabs = () => {
  store.commit('tabsView/closeAllTabs')
  router.push({
    name: 'dashboard-welcome'
  })
}
// 关闭左侧
const closeLeftTabs = () => {
  store.commit('tabsView/closeLeftTabs', getTargetTab())
  checkCurrentTab()
}
// 关闭右侧
const closeRightTabs = () => {
  store.commit('tabsView/closeRightTabs', getTargetTab())
  checkCurrentTab()
}
// 关闭其他
const closeOtherTabs = () => {
  store.commit('tabsView/closeOtherTabs', getTargetTab())
  checkCurrentTab()
}
</script>

<style lang="scss" scoped>
.context-menu {
  position: fixed;
  background-color: #fff;
  list-style: none;
  padding: 2px 0;
  border-radius: 2px;
  box-shadow: 0 2px 8px #00000026;
  box-sizing: border-box;
  outline: none;
  z-index: 300;

  .menu-item {
    width: 80px;
    text-align: center;
    line-height: 40px;
    cursor: pointer;

    &:hover {
      background-color: #f2f2f2;
    }
  }
}
</style>
