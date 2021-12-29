<template>
  <el-form :model="user" label-width="100px" size="small">
    <br>
    <h2 align="center">{{user.name}},你正在更改个人信息 </h2>
    <br>
    <br>
    <el-form-item label="用户姓名"><el-input v-model="user.name"></el-input></el-form-item>
    <el-form-item label="用户密码"><el-input v-model="user.password"></el-input></el-form-item>
    <el-form-item label="用户所在学院"><el-input v-model="user.institute"></el-input></el-form-item>
    <br><br>
    <el-form-item>
      <div align="center">
        <el-button type="primary" @click="update">更新</el-button>
        <el-button @click="goBack">返回</el-button>
      </div>
    </el-form-item>
  </el-form>

</template>

<script>
  const OK = 200;
  export default {
    data() {
      return {
        user:{
          id:'',
          name:'',
          institute:'',
          password:'',
          account:'',
        },
        activity:{},

      }
    },
    methods: {
      get: function(id) {
        this.$axios.get('/api/activities/' + id).then(res => {
          console.log(res.data);
          if (res.data.code == OK) {
            this.userActivity.activityName = res.data.data.activityName;
            this.userActivity.activityId = res.data.data.id;
            this.userActivity.holdTime = res.data.data.holdTime;
            this.userActivity.site = res.data.data.site;
            this.userActivity.introduce = res.data.data.introduce;
          } else {
            this.$message.error(res.data.message, { icon: 5 });
          }
        });
      },


      goBack: function() {
        this.$router.back(-1);
      },

      update: function() {
        console.log(this.user);
        this.$axios.put('/api/users/', this.user).then(res => {
          console.log(this.user);
          // this.$layer.msg(res.data);
          console.log(res.data);
          if (res.data.code == OK) {
            this.$message({
              message: '更新用户成功',
              type: 'success'
            });
          } else {
            this.$message({
              message: res.data.message,
              type: 'error'
            });
          }
        });
      },

      getUser: function(token) {
        this.$axios
          .get('/api/users/getUser', {
            params: {
              token: token
            }
          })
          .then(res => {
            if (res.data.code == OK) {
              this.user.name=res.data.data.name;
              this.user.institute=res.data.data.institute;
              this.user.password=res.data.data.password;
              this.user.account=res.data.data.account;
              this.user.id=res.data.data.id;


              console.log("接下来打印account");
              console.log("接下来打印account");
              console.log("接下来打印account");
              console.log(this.userActivity.account);
            } else {

              this.$message.error(res.data.message);
            }
          });
      },
    },



    created() {

      var token = this.$cookies.get('token');
      console.log(token);
      this.getUser(token);
      this.get(id);
      // try读取当前用户


    },
    watch: {
      $route(to, from) {
        this.getActivityTypeList();
      }
    }
  };
</script>

<style scoped="scoped">
  .show-image {
    width: 350px;
    height: 220px;
  }
  .login-container {
    position: absolute;

  }
</style>
