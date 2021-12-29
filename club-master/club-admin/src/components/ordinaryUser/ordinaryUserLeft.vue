<template>
  <el-menu default-active="1" background-color="grey" text-color="#fff" active-text-color="#5FB878" class="leftNav" :unique-opened="true">

    <el-submenu index="1">
      <template slot="title">
        <i class="el-icon-menu"></i>
        活动管理
      </template>
      <el-menu-item index="1-1"><router-link :to="{ name: 'ordinaryActivity' }" tag="div">活动列表</router-link></el-menu-item>
      <el-menu-item index="1-2"><router-link :to="{ name: 'ordinaryMyActivity' }" tag="div">我的活动</router-link></el-menu-item>
    </el-submenu>

    <el-submenu index="2">
      <template slot="title">
        <i class="el-icon-menu"></i>
        用户管理
      </template>
      <el-menu-item index="2-1"><router-link :to="{ name: 'ordinaryMyInformation' }" tag="div">更改信息</router-link></el-menu-item>
    </el-submenu>

    <el-submenu index="3">
      <template slot="title">
        <i class="el-icon-menu"></i>
        其他协会成员
      </template>
      <el-menu-item index="3-1"><router-link :to="{ name: 'ordinaryUserList' }" tag="div">全部协会会员</router-link></el-menu-item>
    </el-submenu>

    <el-submenu index="4">
      <template slot="title">
        <i class="el-icon-menu"></i>
        协会首页
      </template>
      <el-menu-item index="4-1"><a href="http://localhost:80" target="_blank" tag="div">返回协会首页</a></el-menu-item>
    </el-submenu>
  </el-menu>
</template>

<script>
  const OK = 200
  export default {
    data() {
      return {
        activeIndex: '1',
        accessFrontUrl: 'http://localhost:8011/'
      };
    },
    methods: {
      getFronUrl: function() {
        this.$axios.get('/api/system/frontUrl').then(res => {
          if (res.data.code == OK) {
            this.accessFrontUrl = res.data.data;
          }
        });
      }
    },
    created: function() {
      this.getFronUrl();
    }
  };
</script>

<style scoped="scoped">
  .leftNav {
    position: fixed;
    left: 0;
    top: 0;
    bottom: 0;
    width: 200px;
    background-color:#cbcbb3;
    padding-top: 57px;
  }
  .leftNav .el-menu-item a {
    color: #eee;
    text-decoration: none;
  }
  .router-link-exact-active.router-link-active {
    font-size: 120%;
    color: #409eff !important;
  }
</style>
