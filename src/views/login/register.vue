<!--
 * @Author       : Nimo
 * @FilePath     : /jsd-management/src/views/login/register.vue
 * @Date         : 2022-02-17 13:18:24
 * @LastEditTime : 2022-06-28 13:29:34
 * @LastEditors  : Nimo
 * @Logs         : 
 * 
-->
<template>
  <div>
    <el-form ref="registerForm" :model="form" :rules="rules" label-width="80px" class="login-box">
      <h3 class="login-title">欢迎注册</h3>
      <el-form-item label="账号" prop="username">
        <el-input type="text" placeholder="请输入账号" v-model="form.username" />
      </el-form-item>
      <el-form-item label="邮箱" prop="email">
        <el-input type="text" placeholder="请输入邮箱" v-model="form.email" />
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" placeholder="请输入密码" v-model="form.password" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" v-on:click="onSubmit('registerForm')">注册</el-button>
      </el-form-item>
    </el-form>

    <!-- handleClose -->
    <el-dialog title="温馨提示" :visible.sync="dialogVisible" width="30%">
      <span>请输入账号、邮箱和密码</span>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      form: {
        username: '',
        password: ''
      },
      // 表单验证，需要在 el-form-item 元素中增加 prop 属性
      rules: {
        username: [{ required: true, message: '账号不可为空', trigger: 'blur' }],
        email: [{ required: true, message: '邮箱不可为空', trigger: 'blur' }],
        password: [{ required: true, message: '密码不可为空', trigger: 'blur' }]
      },

      // 对话框显示和隐藏
      dialogVisible: false
    }
  },
  created() {},
  methods: {
    onSubmit(formName) {
      // 为表单绑定验证功能
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.getRegister()
        } else {
          this.dialogVisible = true
          return false
        }
      })
    },

    /**
     * @description: 注册
     */
    getRegister() {
      this.$api
        .fetch('query_user_jsons', {
          username: this.form.username,
          email: this.form.email,
          password: this.form.password
        })
        .then(() => {
          this.$message.success('注册成功')
          this.$router.go(-1)
        })
        .catch(msg => {
          this.$message.error(msg)
        })
    }
  }
}
</script>

<style lang="less" scoped>
.login-box {
  border: 1px solid #dcdfe6;
  width: 350px;
  margin: 180px auto;
  padding: 35px 35px 15px 35px;
  border-radius: 5px;
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  box-shadow: 0 0 25px #ceced1;
}

.login-title {
  text-align: center;
  margin: 0 auto 40px auto;
  color: #303133;
}
</style>
