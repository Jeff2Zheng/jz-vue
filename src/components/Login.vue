<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像区 -->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="avatar" />
      </div>
      <!-- 登录表单 -->
      <div>
        <el-form
          ref="loginFormData"
          :model="loginForm"
          :rules="loginFormRules"
          label-width="60px"
          class="login_form"
        >
          <el-form-item label="账号" prop="userName">
            <el-input v-model="loginForm.userName" prefix-icon="iconfont icon-user"></el-input>
          </el-form-item>
          <el-form-item label="密码" prop="password">
            <el-input
              v-model="loginForm.password"
              type="password"
              prefix-icon="iconfont icon-3702mima"
            ></el-input>
          </el-form-item>
          <el-form-item class="btns">
            <el-button type="primary" @click="login">登录</el-button>
            <el-button type="info" @click="resetLoginForm">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      loginForm: {
        userName: 'xxj',
        password: 'xiaoxiaojiang'
      },
      // 表单验证
      loginFormRules: {
        userName: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 2, max: 10, message: '长度在 2 到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入用户密码', trigger: 'blur' },
          { min: 6, max: 18, message: '长度在 6 到 18 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 表单重置按钮
    resetLoginForm () {
      this.$refs.loginFormData.resetFields()
    },
    login () {
      this.$refs.loginFormData.validate(async valid => {
        if (valid) {
          const { data: res } = await this.$http.post('api-user/api/auth/login', this.loginForm)
          if (res.code !== 200) return this.$message.error(res.message)
          this.$message.success('登录成功')
          // 1、将登陆成功之后的token, 保存到客户端的sessionStorage中; localStorage中是持久化的保存
          //   1.1 项目中出现了登录之外的其他API接口，必须在登陆之后才能访问
          //   1.2 token 只应在当前网站打开期间生效，所以将token保存在sessionStorage中
          sessionStorage.setItem('token', res.data.token)
          sessionStorage.setItem('user', JSON.stringify(res.data))
          // console.log(sessionStorage.getItem("user"));
          // 2、通过编程式导航跳转到后台主页, 路由地址为：/home
          this.$router.push('/home')
        }
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_container {
  background-image: url('../assets/login.jpg');
  display: flex;
  flex-direction: column;
  background-repeat: no-repeat;
  background-size: 100%;
  height: 100vh;
  background-color: #f0f2f5;
  overflow: hidden;
  align-items: center;
  justify-content: center;
}
.login_box {
  width: 450px;
  height: 360px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  -webkit-transform: translate(-50%, -50%);
  background-color: #fff;

  .avatar_box {
    width: 130px;
    height: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 0 0 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
}
.login_form {
  position: absolute;
  bottom: 60px;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
.btns {
  display: flex;
  justify-content: center;
}
.info {
  font-size: 13px;
  margin: 10px 15px;
}
</style>
