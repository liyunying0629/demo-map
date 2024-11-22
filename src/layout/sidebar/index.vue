<script lang="ts" setup>
import { ref } from 'vue'
import SideBarItem from './sidebarItem.vue'
import { routerList } from './data.js'; // 导入数据

const isCollapse = ref(false)

</script>
<template>
  <div class="sidebar">
    <!-- 导航菜单 -->
    <el-menu :router="true" default-active="2" class="el-menu-vertical-demo" :collapse="isCollapse">
      <SideBarItem :routerList="routerList" />
    </el-menu>

  </div>
</template>
<style lang="scss" scoped>
.sidebar {
  height: 100%;
  padding-top: 30px;

  :deep(.el-menu) {
    // width: 100%;
    height: 100%;
    overflow-y: auto;
    background: transparent;
    border: none;

    .el-menu-item {
      font-size: 16px;
      height: 25px;
      padding: 0 30px;
      margin-bottom: 40px;
      background: transparent;
      display: flex;
      align-items: center;
      outline: none;
      box-sizing: border-box;

      &:last-child {
        margin-bottom: 0;
      }

      img {
        width: 24px;
        height: 24px;
        margin-right: 10px;
      }

      span {
        color: var(--vt-main-color);
      }

      &.is-active {
        position: relative;

        span {
          color: var(--vt-main-color);
          font-weight: 600;
          font-family: 'PingFangSC-Semibold';
        }

        &::after {
          content: '';
          position: absolute;
          width: 4px;
          height: 30px;
          background: #2b5ae8;
          left: 0;
          top: 0;
          bottom: 0;
          margin: auto;
          z-index: 999;
        }
      }
    }

    .el-sub-menu {
      font-size: 14px;
      margin-bottom: 40px;

      .el-menu-item {
        min-width: 179px;
        padding: 0 64px !important;
        outline: none;
        overflow: hidden;
        font-size: 14px;
        margin-bottom: 20px;

        &:last-child {
          margin-bottom: 0;
        }

        &:first-child {
          margin-top: 20px;
        }

        span {
          color: #797979;
        }

        &.is-active {
          span {
            font-family: 'PingFangSC-Semibold';
            font-weight: 600;
            color: var(--vt-main-color);
          }
        }
      }

      &.is-opened {
        .el-submenu__title {
          >span {
            color: var(--vt-main-color);
          }
        }

        &.is-active {
          .el-submenu__title {
            font-weight: 600;
          }
        }
      }

      .el-sub-menu__title {
        font-size: 16px;
        height: 25px;
        display: flex;
        align-items: center;
        padding: 0 30px !important;
        background: transparent;

        img {
          width: 24px;
          height: 24px;
          margin-right: 10px;
        }

        &:hover {
          background: transparent;
        }

        +.el-menu {
          .el-submenu__title {
            background: transparent;
            padding: 0 64px !important;
          }

          .el-submenu {
            .el-menu {
              .el-menu-item {
                padding: 0 100px !important;

                &.is-active {
                  padding: 0 100px !important;
                }
              }
            }
          }
        }

        // span {
        //   position: relative;
        //   &::after {
        //     content: '';
        //     @include imageURL('closeMenu.png');
        //     width: vw(5 * 2);
        //     height: vw(5 * 2);
        //     position: absolute;
        //     right: vw(-15 * 2);
        //     top: 0;
        //     bottom: 0;
        //     margin: auto;
        //   }
        // }
      }

      .el-sub-menu__icon-arrow {
        display: none;
      }

      &.is-opened {
        >.el-sub-menu__title {
          &:hover {
            background: transparent;
          }
        }

        .el-menu-item {
          padding: 0 64px !important;

          &.is-active {
            padding-left: 64px !important;
            font-weight: 600;
            color: var(--vt-main-color);
          }
        }
      }

      .el-submenu {
        margin-bottom: 20px;

        &:first-child {
          margin-top: 20px;
        }
      }
    }
  }
}
</style>
