<template>
  <div class="login_container">
    <div class="login_box">
      <div class="avatar_box">
        <!--头像-->
        <img src="../assets/白兔赤乌.png" alt/>
      </div>
      <!--添加表单-->
      <el-form
          ref="loginFormRef"
          :model="loginForm"
          :rules="loginRules"
          class="login_form"
          label-width="0px"
      >
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="iconfont icondenglu"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="iconfont iconmima" type="password"></el-input>
        </el-form-item>
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
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
        password: "123456"
      },
      loginRules: {
        username: [
          {required: true, message: "请输入用户名", trigger: "blur"},
          {min: 2, max: 18, message: "长度在2 到 18 个字符", trigger: "blur"}
        ],
        password: [
          {required: true, message: "请输入密码", trigger: "blur"},
          {min: 6, max: 10, message: "密码为 6~10 位", trigger: "blur"}
        ]
      }
    };
  },
  methods: {
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields();
    },
    login() {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return;
        // 调用get请求
        const {data: res} = await this.$http.post("login", this.loginForm);
        if (res == "ok") {
          window.sessionStorage.setItem('flag', 'ok'); // session 放置
          this.$message.success("登陆成功！！！");
          this.$router.push({path: "/home"});
        } else {
          this.$message.error("登录失败！！！");
        }
      });
    }
  }
};
</script>

<style lang="less" scoped>

// 跟节点样式
.login_container {
  //background-color: #2b4b6b;
  height: 100%;
  background: url("../assets/bg.jpg");
  background-size: 100%;
}

// 登录框
.login_box {
  width: 450px;
  height: 300px;
  background-image: linear-gradient(to bottom right, #c6ffdd, #fbd786, #f7797d);
  border-radius: 25px; // 圆角
  position: absolute; // 绝对定位
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%); // 根据自己位置 以自己为长度位移
  // 头像框
  .avatar_box {
    width: 130px;
    height: 130px;
    border: 1px solid #eee;
    border-radius: 50%; // 加圆角
    padding: 10px;
    box-shadow: 0 0 10px #ddd; // 盒子阴影
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-image: linear-gradient(to bottom right, #40e0d0, #ff8c00, #ff0080);

    img {
      width: 100%;
      height: 100%;
      border-radius: 50%; // 加圆角
      background-color: #eee;
    }
  }

  .btns {
    display: flex; // 弹性布局
    justify-content: flex-end; // 尾部对齐
  }

  .login_form {
    position: absolute;
    bottom: 0%;
    width: 100%;
    padding: 0 10px;
    box-sizing: border-box; // 设置边框
  }
}
</style>
