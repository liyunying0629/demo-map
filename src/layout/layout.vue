<template>
  <div id="app-main">
    <router-view v-slot="{ Component, route }">
      <transition name="router-fade" mode="out-in">
        <div key="route.fullPath">
          <component :is="Component" />
        </div>
      </transition>
    </router-view>
  </div>
</template>
<script setup lang="ts"></script>
<style lang="scss">
#app-main {
  flex: 1;
  height: 100%;
  padding-top: 73px;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  -ms-flex-direction: column;
  flex-direction: column;
  padding-left: 40px;
}
</style>
