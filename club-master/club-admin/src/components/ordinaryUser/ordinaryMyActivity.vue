<template>
  <div v-if="activityPage != null">

    <el-table :data="activityData" stripe style="width:100%" border size="mini">
      <el-table-column prop="id" label="活动id" width="150"></el-table-column>
      <el-table-column prop="activityName" label="活动名称" width="350" :sortable="true"></el-table-column>
      <el-table-column prop="site" label="活动地点" width="350" :sortable="true"></el-table-column>
      <el-table-column prop="holdTime" label="活动举行时间" :sortable="true"></el-table-column>

    </el-table>
    <el-pagination
      class="page"
      background
      layout="total, sizes, prev, pager, next"
      :current-page.sync="currentPage"
      :total="activityPage.total"
      @current-change="refreshactivityPage"
      @size-change="handleSizeChange"
      :page-size="GLOBAL.pageSize"
      :page-sizes="GLOBAL.pageSizeArray"
    ></el-pagination>
  </div>
</template>

<script>
  const OK = 200;
  export default {
    data() {
      return {
        searchAccount: '',
        currentPage: 1,
        activityPage: {},
        activityData: [],
        activitySearch: {},
        activityName: null,
        activitySite: null,
      };
    },
    components: {  },
    methods: {

  getActivityPage: function(pageNum, pageSize,) {
        this.$axios
          .get('/api/userActivity', {
            params: {
              pageNum: pageNum,
              pageSize: pageSize,
              search: this.searchAccount,
            }
          })
          .then(res => {
            if (res.data.code == OK) {
              this.activityPage = res.data.data;
              this.activityData = this.activityPage.list;
              console.log("get请求方法打印search");
              console.log(this.searchAccount);
              console.log("get请求方法结束");


            } else {
              this.$message.error(res.data.data);
              console.log("code不是200");
            }
          });
      },

      getUser: function(token) {
        this.$axios
          .get('/api/users/getUser', {
            params: {
              token: token,
            }
          })
          .then(res => {
            if (res.data.code == OK) {
              this.searchAccount = res.data.data.account;
              this.getActivityPage(this.currentPage, 8);
              console.log("getUser方法中打印searchaccount");
              console.log(this.searchAccount);
              console.log("getUser方法中打印searchaccount结束");


            } else {
              this.$message.error(res.data.message);
            }
          });
      },


      refreshactivityPage: function(page) {
        this.currentPage=page;
        this.getActivityPage(this.currentPage, this.pageSize);
      },

      handleSizeChange:function(size){
        this.pageSize =size
        this.getActivityPage(this.currentPage, this.pageSize);
      },

    },

    created() {
      var token = this.$cookies.get('token');
   this.getUser(token);
   console.log("creted方法中打印searchAccount");
   console.log(this.searchAccount);
   console.log("create结束打印");





    },


    watch: {
      $route(to, from) {
        this.getActivityPage(this.currentPage, 8, this.searchAccount);
      }
    }
  };
</script>
<style scoped="scoped">
  @import "../../css/common.css";
</style>
