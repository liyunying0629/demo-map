<script lang="ts" setup>

defineProps({
  routerList: {
    type: Array,
    default: () => {
      return []
    }
  }
})
</script>
<template>
  <template v-for="menu in routerList">
    <el-sub-menu :key="menu.id" :index="menu.id" v-if="menu.children && menu.children.length > 0 && !menu.hidden">
      <template #title>
        <el-icon>
          <ChromeFilled />
        </el-icon>
        <span class="menu-title">{{ menu.name }}</span>
      </template>
      <sidebarItem :router-list="menu.children"></sidebarItem>
    </el-sub-menu>
    <el-menu-item :route="menu.path" :key="menu.id" :index="menu.id" v-else-if="!menu.hidden">
      <span class="menu-title">{{ menu.name }}</span>
    </el-menu-item>
  </template>
</template>
