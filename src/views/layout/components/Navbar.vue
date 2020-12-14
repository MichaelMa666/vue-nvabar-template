<template>
  <div class="navbar">
    <div class="left-menu">
      <!-- logo -->
    </div>
    <div class="center-menu">
      <el-menu
        :default-active="activeMenu"
        background-color="#5a5e66"
        text-color="#fff"
        :unique-opened="false"
        active-text-color="#ffd04b"
        :collapse-transition="false"
        mode="horizontal"
        menu-trigger="hover"
      >
        <menu-item v-for="route in routes" :key="route.path" :item="route" :base-path="route.path" />
      </el-menu>
    </div>
    <div class="right-menu">
      <el-dropdown class="avatar-container right-menu-item hover-effect" trigger="click">
        <div class="avatar-wrapper">
          <img :src="'https://wpimg.wallstcn.com/f778738c-e4f8-4870-b634-56703b4acafe.gif'+'?imageView2/1/w/80/h/80'" class="user-avatar">
          <i class="el-icon-caret-bottom" />
        </div>
        <el-dropdown-menu slot="dropdown">
          <router-link to="/">
            <el-dropdown-item>
              {{ '首页' }}
            </el-dropdown-item>
          </router-link>
          <a target="_blank" href="https://github.com/PanJiaChen/vue-element-admin/">
            <el-dropdown-item>
              {{ '项目地址' }}
            </el-dropdown-item>
          </a>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </div>
</template>

<script>
import path from 'path'
import { constantRoutes } from '@/router'
import MenuItem from './MenuItem.vue'
export default {
  
  components: {
    MenuItem
  },
  data() {
    return {
      routes: constantRoutes,
      isNest:false
    }
  },
  watch: {
    routes: {
      deep: true,
      immediate: true,
      handler(val) {
        console.log(val)
      }
    }
  },
  computed: {
    activeMenu() {
      const route = this.$route
      const { meta, path } = route
      // if set path, the sidebar will highlight the path you set
      if (meta.activeMenu) {
        return meta.activeMenu
      }
      return path
    },
  },
  methods: {
    hasOneShowingChild(children = [], parent) {
      const showingChildren = children.filter(item => {
        if (item.hidden) {
          return false
        } else {
          // Temp set(will be used if only has one showing child)
          this.onlyOneChild = item
          return true
        }
      })

      // When there is only one child router, the child router is displayed by default
      if (showingChildren.length === 1) {
        return true
      }

      // Show parent if there are no child router to display
      if (showingChildren.length === 0) {
        this.onlyOneChild = { ... parent, path: '', noShowingChildren: true }
        return true
      }

      return false
    },
    resolvePath(route, routePath) {
      // if (isExternal(routePath)) {
      //   return routePath
      // }
      return path.resolve(route.path, routePath)
    },
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
.navbar {
  height:60px;
  overflow: hidden;
  background-color: #5a5e66;
  .left-menu {
    height: 100%;
    line-height: 60px;
    width: 200px;
    display: inline-block;
  }
  .center-menu {
    display: inline-block;
  }
  .right-menu {
    float: right;
    width: 100px;
    height: 100%;
    line-height: 50px;
    &:focus {
      outline: none;
    }

    .right-menu-item {
      display: inline-block;
      padding: 0 8px;
      height: 100%;
      font-size: 18px;
      color: #5a5e66;
      vertical-align: text-bottom;

      &.hover-effect {
        cursor: pointer;
        transition: background .3s;

        &:hover {
          background: rgba(0, 0, 0, .025)
        }
      }
    }

    .avatar-container {
      margin-right: 30px;

      .avatar-wrapper {
        margin-top: 5px;
        position: relative;

        .user-avatar {
          cursor: pointer;
          width: 40px;
          height: 40px;
          border-radius: 10px;
        }

        .el-icon-caret-bottom {
          cursor: pointer;
          position: absolute;
          right: -20px;
          top: 25px;
          font-size: 12px;
        }
      }
    }
  }
}
</style>
