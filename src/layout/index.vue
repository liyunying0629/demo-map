<template>
  <div class="page-box">
    <div class="page-left">
      <div class="logo-box">
        <img src="../../public/favicon.ico" alt="" />
      </div>
      <sidebar class="side-menu"></sidebar>
    </div>
    <div class="page-right">
      <div class="header-box">
        <RouterView />
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import sidebar from './sidebar/index.vue'
import { RouterLink, RouterView } from 'vue-router'
</script>
<style lang="scss" scoped>
.page-box {
  width: 100%;
  height: 100%;
  display: flex;
  overflow: hidden;
  position: relative;

  .page-left {
    max-width: 258px;
    position: fixed;
    left: 0px;
    top: 0;
    bottom: 0;
    margin: auto;
    display: flex;
    flex-direction: column;
    background: #f0f0f0;

    .logo-box {
      height: 73px;
      display: flex;
      align-items: center;
      margin: 0 auto;

      img {
        width: 33px;
      }
    }

    .side-menu {
      width: 100%;
      flex: 1;
      background: url('@/assets/menu/bg.png') no-repeat;
      background-size: 100% 100%;
      overflow: hidden;
      transition: width 0.28s;
      margin-bottom: 60px;
      box-sizing: content-box;
    }
  }

  .page-right {
    flex: 1;
    height: 100%;
    transition: margin-left 0.28s;
    margin-left: 264px;
    display: flex;
    flex-direction: column;

    .header-box {
      position: fixed;
      width: 87.0625%;
      //210-230
      left: 260px;
      top: 0;
      padding: 0 50px;
      display: flex;
      align-items: center;
      height: 73px;
      justify-content: space-between;
      box-sizing: border-box;
      z-index: 9;
    }
  }
}
</style>
