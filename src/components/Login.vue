<template>
  <div class="login_bg">
    <div class="login_box">
      <!--      头像-->
      <div class="avatar_box">
        <img src="../assets/logo.png">
      </div>
      <!--      登录区域-->
      <el-form ref="loginFromRef" :model="loginForm" :rules="loginFromRules" label-width="0px" class="login_form">
        <!--        用户名-->
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="el-icon-user" placeholder="请输入用户名"></el-input>
        </el-form-item>
        <!--密码-->
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="el-icon-lock" type="password"
                    placeholder="请输入密码"></el-input>
        </el-form-item>
        <!--        按钮-->
        <el-form-item class="btn_sty">
          <el-button type="primary" @click="startLogin">登录</el-button>
          <el-button type="info" @click="resetLoginFrom">重置</el-button>
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
      loginForm: {
        username: '',
        password: ''
      },
      // TODO 表单验证规则
      loginFromRules: {
        username: [
          {
            required: true,
            message: '请输入用户名',
            trigger: 'blur'
          },
          {
            min: 3,
            max: 10,
            message: '长度在 3 到 10 个字符',
            trigger: 'blur'
          }
        ],
        password: [
          {
            required: true,
            message: '请输入密码',
            trigger: 'blur'
          },
          {
            min: 3,
            max: 10,
            message: '长度在大于6个字符',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    // 点击重置按钮
    resetLoginFrom () {
      this.$refs.loginFromRef.resetFields()
    },
    startLogin () {
      const mThis = this
      mThis.$refs.loginFromRef.validate(async function (valid) {
        if (!valid) return false
        const { data: rets } = await mThis.$http.post('login', mThis.loginForm)
        console.log(rets)
        if (rets.meta.status !== 200) {
          return mThis.$message.error(rets.meta.msg)
        } else {
          mThis.$message.success(rets.meta.msg)
          // 保存token
          window.sessionStorage.setItem('token', rets.data.token)
          // 通过编程式导航跳转到指定页面
          mThis.$router.push('/home')
        }
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_bg {
  background-color: #2b4b6b;
  height: 100%;
}

.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);

  .avatar_box {
    height: 130px;
    width: 130px;
    border: 1px solid #eeeeee;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 0 0 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #ffffff;

    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eeeeee;
    }
  }
}

.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}

.btn_sty {
  display: flex;
  justify-content: flex-end;
}
</style>
