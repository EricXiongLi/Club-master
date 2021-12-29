<template>
	<el-form class="" :model="user" label-width="100px">
		<el-form-item label="用户名称"><el-input v-model="user.name"></el-input></el-form-item>
    <el-form-item label="用户账号"><el-input v-model="user.account"></el-input></el-form-item>
    <el-form-item label="用户密码"><el-input v-model="user.password"></el-input></el-form-item>
    <el-form-item label="用户所在学院"><el-input v-model="user.institute"></el-input></el-form-item>


    <el-form-item>
			<el-button type="primary" @click="add">添加</el-button>
			<el-button>取消</el-button>
		</el-form-item>
	</el-form>
</template>

<script>
const OK = 200;
export default {
	data() {
		return {
			user: {},
			userTypeList: [],
			editorOption: {
				placeholder: '输入会员所在学院：',
				// 编辑器的配置
				// something config
				theme: 'snow'
			},
		};
	},
	methods: {
		add: function() {
			console.log(this.user);
			this.$axios.post('/api/users/', this.user).then(res => {
				console.log(this.user);
				// this.$layer.msg(res.data);
				console.log(res.data);
				if (res.data.code == OK) {
					this.$message({
						message: '添加用户成功',
						type: 'success'
					});
          this.$router.push({ name: 'UserList' });

        }
				else {
					this.$message({
						message: res.data.message,
						type: 'error'
					});
				}
			});
		},
		getuserTypeList: function() {
			this.$axios.get('/api/userTypes').then(res => {
				if (res.data.code == OK) {
					this.userTypeList = res.data.data;
				} else {
					this.$message.error(res.data.data);
				}
			});
		},

	},
	created() {
		this.getuserTypeList();
	},
	watch: {
		$route(to, from) {
			this.getuserTypeList();
		}
	}
};
</script>

<style scoped="scoped">

</style>
