<template>
  <el-container class="container">
    <el-aside
      class="aside"
      width="auto"
    >
      <Aside class="aside-menu" :isCollapse="isCollapse"></Aside>
    </el-aside>
    <el-container>
      <el-header class="header">
        <Header @isOnOrOff="isOnOrOff" :isCollapse="isCollapse"></Header>
      </el-header>
      <el-main>
        <router-view />
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import Aside from 'layout/components/Aside.vue/'
import Header from 'layout/components/Header.vue/'
export default {
  name: 'Layout',
  components: {
    Aside,
    Header
  },
  data () {
    return {
      isCollapse: false
    }
  },
  methods: {
    isOnOrOff () {
      this.isCollapse = !this.isCollapse
    }
  }
}
</script>

<style scoped lang="less">
.container {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  .aside {
    background-color: #001e34;
    .aside-menu {
      height: 100%;
    }
  }

  .header {
    border-bottom: 2px solid #eee;
    background-color: #fff;
  }
}

</style>
