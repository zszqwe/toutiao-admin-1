<template>
    <el-col>
      <h5>logo</h5>
      <el-menu
        :default-active="$route.path"
        class="el-menu-vertical-demo menu"
        background-color="#001e34"
        text-color="#fff"
        active-text-color="#ffd04b"
        :collapse="isCollapse"
        router
      >
        <el-menu-item index="/">
          <i class="el-icon-s-home"></i>
          <span slot="title">首页</span>
        </el-menu-item>
        <el-menu-item index="/article">
          <i class="el-icon-document"></i>
          <span slot="title">内容管理</span>
        </el-menu-item>
        <el-menu-item index="/image">
        <i class="el-icon-picture"></i>
        <span slot="title">素材管理</span>
      </el-menu-item>
        <el-menu-item index="/publish">
          <i class="el-icon-s-promotion"></i>
          <span slot="title">发布文章</span>
        </el-menu-item>
        <el-menu-item index="/comment">
          <i class="el-icon-s-comment"></i>
          <span slot="title">评论管理</span>
        </el-menu-item>
        <el-submenu index="/fans">
          <template slot="title">
            <i class="el-icon-present"></i>
            <span slot="title">粉丝管理</span>
          </template>
          <el-menu-item-group>
            <el-menu-item index="/fans/list">粉丝列表</el-menu-item>
            <el-menu-item index="/fans/show">粉丝数据统计</el-menu-item>
          </el-menu-item-group>
        </el-submenu>
        <el-menu-item index="/settings">
          <i class="el-icon-setting"></i>
          <span slot="title">个人设置</span>
        </el-menu-item>
      </el-menu>
    </el-col>
</template>

<script>
export default {
  name: 'Aside',
  data () {
    return {
    }
  },
  props: {
    isCollapse: {
      type: Boolean,
      default: false
    }
  },
  methods: {
  }
}
</script>

<style scoped lang="less">
.menu {
  border: 0;
}

.el-menu-vertical-demo:not(.el-menu--collapse) {
  width: 200px;
  min-height: 400px;
}
</style>
