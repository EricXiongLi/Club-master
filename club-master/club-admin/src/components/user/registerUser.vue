<template>
  <div class="login-container">
    <el-form :model="user" status-icon ref="user" label-width="100px" class="loginForm">
      <h1 class="login-title">会员注册</h1>
      <br/>
      <br/>
      <el-form-item label="姓名">
        <el-input v-model="user.name" placeholder="请输入真实姓名"></el-input>
      </el-form-item>
      <el-form-item label="账号">
        <el-input v-model="user.account" placeholder="请输入你的邮箱"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="pass">
        <el-input placeholder="请输入密码" type="password" v-model="user.password"></el-input>
      </el-form-item>
      <el-form-item label="所在学院">
        <el-input v-model="user.institute" placeholder="所在学院"></el-input>
      </el-form-item>
      <el-form-item label="验证码">
        <el-input v-model="user.code" placeholder="请输入验证码" autocomplete="off"></el-input>
        <img :src="captchaUrl" alt="验证码" @click="refreshCode"/>
      </el-form-item>
      <router-link :to="{ name: 'login' }">已有账号？立即登录</router-link>
      <el-form-item>
        <el-button type="primary" @click="register">注册</el-button>
        <el-button @click="resetForm('user')">重置</el-button>
      </el-form-item>
    </el-form>

  </div>
</template>

<script>
  const OK = 200;
  export default {
    data() {
      return {
        rules: {},
        user: {},
        code: '',
        token: '',
        captchaUrl: ''
      };
    },
    methods: {
      register: function () {
        console.log(this.user);
        this.$axios.post('/api/users/', this.user).then(res => {
          console.log(this.user);
          console.log(res.data);
          if (res.data.code == OK) {
            this.$message({
              message: '恭喜，注册成功',
              type: 'success'
            });
          } else {
            this.$message({
              message: res.data.message,
              type: 'error'
            });
          }
        });
        this.$axios.post('/api/users/login', this.user)
          .then(res => {
            console.log(this.user);
            console.log(res.data);
            if (res.data.code == OK) {
              this.$message.success('登录成功');
              this.$cookies.set('token', res.data.data, 60 * 30);
              this.token = res.data.data;
              console.log(this.token);
              this.$router.push({name: 'ordinaryHome'});
            } else {
              this.$message.error(res.data.message);
            }
          })
          .catch(function (error) {
            this.$message.error(error);
          });
      },


      refreshCode: function () {
        this.captchaUrl = '/api/users/getKaptcha?time=' + new Date().getTime();
        console.log('更新' + this.captchaUrl);
      },

      resetForm: function () {

      },
      tryToAminPage: function () {
        var token = this.$cookies.get('token');
        if (token != null) this.$router.push({name: 'Home'});
      },
      test: function () {
        // (this.user.account = 'test@163.com'), (this.user.password = '123456');
      }
    }
    ,
    created() {
      // this.tryToAminPage();
      this.test();
      this.refreshCode();
    }
  }
  ;
</script>

<style scoped="scoped">
  .loginForm {
    width: 400px;
    margin: 100px auto; /* 上下间距200px，左右自动居中*/
    background-color: rgb(255, 255, 255); /* 透明背景色 */
    padding: 70px;
    border-radius: 20px; /* 圆角 */
  }

  .login-container {
    position: absolute;
    width: 100%;
    height: 120%;
    background: url("../../assets/bg.jpg");
  }

  .login-title {
    color: #303133;
    text-align: center;
  }
</style>
