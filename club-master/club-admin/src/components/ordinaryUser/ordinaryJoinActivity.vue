<template>
  <el-form :model="userActivity" label-width="100px" size="small">
    <br>
    <h2 align="center">{{userActivity.account}},你将要参加的活动信息如下，请确认是否确定参加 </h2>
    <br>
    <br>
    <el-form-item label="用户账号"><el-input v-model="userActivity.account" disabled="true"></el-input></el-form-item>
    <el-form-item label="活动id"><el-input v-model="userActivity.activityId" disabled="true"></el-input></el-form-item>
    <el-form-item label="活动名称"><el-input v-model="userActivity.activityName" disabled="true"></el-input></el-form-item>
    <el-form-item label="活动举行时间">
      <el-date-picker type="date" value-format="yyyy 年 MM 月 dd 日" format="yyyy 年 MM 月 dd 日" placeholder="选择日期" v-model="userActivity.holdTime" disabled="true"></el-date-picker>
    </el-form-item>
    <el-form-item label="活动地点"><el-input v-model="userActivity.site" disabled="true"></el-input></el-form-item>
    <quill-editor v-model="userActivity.introduce" :options="editorOption" disabled="true"></quill-editor>
    <br><br>
    <el-form-item>
      <div align="center">
      <el-button type="primary" @click="join">确定</el-button>
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
        userActivity:{
          account:'',
          activityId:'',
          activityName:'',
          holdTime:'',
          site:'',
          introduce:''
        },
        activity:{},
        user: {},
        editorOption: {
          placeholder: '输入活动简介：',
          // 编辑器的配置
          // something config
          theme: 'snow',

        }
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

      join: function() {
        console.log(this.userActivity);
        this.$axios.post('/api/userActivity/', this.userActivity).then(res => {
          console.log(this.userActivity);
          console.log(res.data);
          // if (res.data.code == OK) {
          //   this.$message({
          //     message: '报名成功',
          //     type: 'success'
          //   });
          // } else {
            this.$message({
              message: '报名成功',
              type: 'success',
              // type:'error'
            });
          this.$router.push({name: 'ordinaryActivity'});

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
              this.userActivity.account=res.data.data.account;

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

      var id = this.$route.query.id;
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
