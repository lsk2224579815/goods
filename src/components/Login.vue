<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像区 -->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt />
      </div>
      <!-- 表单区 -->
      <el-form
        ref="loginFormRef"
        label-width="0px"
        :rules="loginFormRules"
        class="login_form"
        :model="loginForm"
      >
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input prefix-icon="iconfont icon-users" v-model="loginForm.username"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input prefix-icon="iconfont icon-3702mima" show-password v-model="loginForm.password"></el-input>
        </el-form-item>
        <!-- 按钮 -->
        <el-form-item class="btns">
          <!-- 登录 -->
          <el-button type="primary" @click="login">登录</el-button>
          <!-- 重置 -->
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginForm: {
        username: "admin",
        password: "123456",
      },
      /* 校验对象 */
      loginFormRules: {
          /* 账号校验 */
        username: [
          { required: true, message: "请输入用户名称", trigger: "blur" },
          {
            min: 4,
            max: 10,
            message: "长度在 4 到 10 个字符",
            trigger: "blur",
          },
        ],
        /* 密码校验 */
        password: [
          { required: true, message: "请输入用户密码", trigger: "blur" },
          {
            min: 6,
            max: 15,
            message: "长度在 6 到 15 个字符",
            trigger: "blur",
          },
        ],
      },
    };
  },
  methods: {
      /* 重置 */
    resetLoginForm() {
       // console.log(this)
       this.$refs.loginFormRef.resetFields();
    },
    login(){
        this.$refs.loginFormRef.validate(valid => {
           if(!valid) return;
           this.$axios.post("login",this.loginForm).then(res => {
             if(res.data.meta.status !== 200) 
              return this.$message.error("登录失败,"+res.data.meta.msg);
               this.$message.success("登录成功");
               /* 1. 将登录成功后的token保存到客户端的sessionStorage中
                    1.1 项目中除了登录之外的API接口,必须在登录之后才能访问
                    1.2 token 只应在当前网站打开期间有效,所以将token保存在sessionStorage中
                  2. 通过编程式导航跳转到后台页面,路由地址式/home
                    
                */
               window.sessionStorage.setItem("token",res.data.data.token);
               this.$router.push('/home')
           })
        })
    }
  },
};
</script>

<style lang="less" scoped>
.login_container {
  background-color: #2b4b6b;
  height: 100%;
}
.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  border-radius: 3px;

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
  bottom: 0;
  width: 100%;
  padding-left: 20px;
  padding-top: 0px;
  padding-right: 20px;
  box-sizing: border-box;
}
.btns {
  display: flex;
  justify-content: flex-end;
}
</style>
