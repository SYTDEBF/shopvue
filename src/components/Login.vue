<template>
 <div class="login_container">
  <div class="login_box">
     <!--头像区域-->
     <div class="avatar_box">
       <img src="../assets/avatar.jpg" alt="">
     </div>
     <!--登陆表单区域-->
    <el-form  ref="loginFormRef" v-bind:model="loginForm" :rules="loginFormRules" class="login_form">
      <el-form-item prop="username">
        <el-input prefix-icon="el-icon-user" v-model="loginForm.username"  clearable placeholder="用户名"></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input type="password" round prefix-icon="el-icon-lock" v-model="loginForm.password" clearable placeholder="密码"></el-input>
      </el-form-item>
      <el-form-item class="btns">
        <el-button type="primary" @click="login" round>登陆</el-button>
        <el-button type="info" @click="resetLoginForm" round>重置</el-button>
      </el-form-item>
    </el-form>
  </div>
 </div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      // 这是登陆表单的数据对象
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      // 表单验证规则
      loginFormRules: {
        // 用户名是否合法
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在3到10字符之间', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 16, message: '长度在6到16字符之间', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 点击重置按钮，重置表单
    resetLoginForm () {
      // console.log(this);
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      // 表单验证，回调的匿名函数 valid参数返回表单验证结果
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登陆失败 ' + res.meta.msg)
        else this.$message.success(res.meta.msg)
        window.sessionStorage.setItem('token', res.data.token)
        await this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
 .login_container{
   background-color: aquamarine;
   height: 100%;
 }
 .login_box{
   width: 450px;
   height: 300px;
   background-color: azure;
   border-radius: 2%;
   position: absolute;
   left: 50%;
   right: 50%;
   transform: translate(-50%,50%);
 }
 .avatar_box{
    height: 110px;
    width: 110px;
    border-radius: 50%;
    border: 1px #eee solid;
    padding: 10px;
    box-shadow: 0 0 8px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%,-50%);
    background-color: #eeeeee;
    img{
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eeeeee;
    }
 }
 .login_form{
   position: absolute;
   bottom: 0;
   width: 100%;
   padding: 0 20px;
   box-sizing: border-box;
 }
 .btns{
   display: flex;
   justify-content: center;
 }

</style>
