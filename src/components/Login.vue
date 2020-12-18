<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像区 -->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="">
      </div>
      <!-- 登录表单区域 -->
      <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" class="login_form">
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" type="password" prefix-icon="el-icon-lock"></el-input>
        </el-form-item>
        <!-- 按钮区 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">提交</el-button>
          <el-button type="info" @click="resetLoginForm()">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      // 这是登录表单的数据绑定对象
      loginForm: {
        username: '',
        password: ''
      },
      loginFormRules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 点击重置按钮 重置登录表单
    resetLoginForm () {
      this.$refs.loginFormRef.resetFields()
    },
    // 登录的预校验
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return false
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败，用户名密码错误！')
        this.$message.success('登录成功！')
        console.log(res)
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
        // 1.将登录成功后的 token，保存到客户端的 sessionStorage 中
        //   项目中除了登录之外的其他API接口，必须在登录之后才能访问
        //   token 只应在当前网站打开期间生效，所以将token保存在sessionStorage中
        // 2.通过编程式导航跳转到后台主页，路由地址是 /home
      })
    }
  }
}
</script>

<style scoped>

.login_container{
  height: 100%;
  position: relative;
  background-color: #2b4b6b;
}

.login_box{
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -225px;
  margin-top: -150px;
}

.avatar_box{
  position: absolute;
  background-color: #fff;
  left: 50%;
  margin-left: -65px;
  margin-top: -65px;
  width: 130px;
  height: 130px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
}

.avatar_box img{
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background-color: #eee;
}

.login_form{
  box-sizing: border-box;
  margin: 100px 20px 0;
}

.btns{
  float: right;
}
</style>
