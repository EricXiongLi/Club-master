<template>
  <div v-if="userPage != null">
    <el-form :inline="true" size="small">
      <el-form-item label="用户名称">
        <el-input placeholder="请输入用户名称" prefix-icon="el-icon-search" v-model="userName" class="input-with-select"
                  width="120px"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="find" icon="el-icon-search">查询</el-button>
      </el-form-item>
    </el-form>
    <el-table :data="userData" stripe style="width:100%" size="mini" border>
      <el-table-column prop="id" label="id" width="100"></el-table-column>
      <el-table-column prop="name" label="用户姓名" width="140" :sortable="true"></el-table-column>
      <el-table-column prop="account" label="用户账号" width="220" :sortable="true"></el-table-column>
      <el-table-column prop="institute" label="学院" width="190" :sortable="true"></el-table-column>
      <el-table-column prop="createTime" label="用户创建时间" width="190" :sortable="true"></el-table-column>
      <el-table-column fixed="right" label="操作" width="360">
        <template slot-scope="scope">
          <el-button type="primary" icon="el-icon-edit" @click="editPage(scope.row)" size="mini">编辑</el-button>
          <el-button type="success" icon="el-icon-plus" @click="addPage" size="mini">添加</el-button>
          <el-button type="danger" icon="el-icon-delete" @click="deleteUser(scope.row)" size="mini">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination class="page" background layout="total, sizes, prev, pager, next" :current-page.sync="currentPage"
                   :total="userPage.total" @current-change="refreshuserPage"
                   :page-size="userPage.pageSize"></el-pagination>
  </div>
</template>

<script>
    const OK = 200;
    export default {
        data() {
            return {
                userPage: {},
                userData: [],
                currentPage: 1,
                userSearch: {},
              userName:null,

              input: '',
              pageSize: this.GLOBAL.pageSize

            };
        },
        components: {},
        methods: {
            getUserPage: function (pageNum, pageSize) {
                this.$axios
                    .get('/api/users', {
                        params: {
                            pageNum: pageNum,
                            pageSize: pageSize,
                          name: this.userName,
                        }
                    })
                    .then(res => {
                        // console.log(res.data.data);
                        if (res!=null && res.data.code == OK) {
                            this.userPage = res.data.data;
                            this.userData = this.userPage.list;
                            // console.log(this.userData);
                        } else {
                            this.$message.error(res.data.data);
                        }
                    });
            },
            editPage: function (row) {
                var id = row.id;
                console.log(row.id);
                this.$router.push({name: 'EditUser', query: {id: id}});
            },
            addPage: function () {
                this.$router.push({name: 'AddUser'});
            },
            deleteDao: function (id) {
                this.$axios.delete('/api/users/' + id).then(res => {
                    if (res.data.code == OK) {
                        // this.userPage = res.data.data;
                        // this.userData = res.data.data.list;
                      this.$message.success('删除成功');

                      console.log(this.userData);
                    } else {
                        this.$message.error(res.data.data);
                    }
                });
            },
            deleteUser: function (row) {
              var id = row.id;
              this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                })
                    .then(() => {
                         this.deleteDao(row.id);
                      this.getUserPage(this.currentPage, this.pageSize);
                    })
                    .catch(() => {
                        this.$message({
                            type: 'info',
                            message: '已取消删除'
                        });
                    });
            },
            refreshuserPage: function () {
                this.getUserPage(this.currentPage, 8);
            },

            find: function () {
                this.getUserPage(this.currentPage, this.pageSize);
            }
        },
        created() {
            this.getUserPage(1, 8);
        },
        watch: {
            $route(to, from) {
                this.getUserPage(this.currentPage, 8);
            }
        }
    };
</script>
<style scoped="scoped">
  @import "../../css/common.css";
</style>
