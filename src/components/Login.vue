<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像 -->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="" />
      </div>
      <!-- 表单登录区域 -->
      <el-form  ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login-form">
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input type="password" v-model="loginForm.password" prefix-icon="iconfont icon-3702mima"></el-input>
        </el-form-item>
        <!-- 按钮区域 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="success" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
export default {
  data(){
    return{
      // 这是表单数据绑定
     loginForm:{
       username:'admin',
       password:'123456'
      },
      // 这是表单的验证规则对象
      loginFormRules:{
        // 验证用户名密码是否合法
        username:[
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        password:[
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 12, message: '长度在 6 到 12 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods:{
    // 重置
    resetLoginForm(){
      console.log(this)//指向loginForm这个实例
      this.$refs.loginFormRef.resetFields();
    },
    login(){
      this.$refs.loginFormRef.validate(async valid=>{
        // 这里的valid是布尔值
        // console.log(valid)
        if(!valid){
          this.$message({
            message:'校验失败',
            type:'error',
            duration:800
          })
          return
        }
        const {data:res} = await this.$http.post("login",this.loginForm)
        if(res.meta.status !==200) return this.$message.error("登录失败")
        this.$message.success("登录成功")
        console.log(res)
        // 保存登录成功后的token
        window.sessionStorage.setItem("token",res.data.token);
        // console.log(res.data.token)
        // 跳转
        this.$router.push('/home')

      });
    }
  }
};
</script>
<style lang="less" scoped>
.login_container {
  background-color: #2b4b6b;
  height: 100%;
  position: relative;
}
.login_box {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 10px;
  position: relative;
}
.avatar_box {
  position: absolute;
  left: 50%;
  top: 0%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  border-radius: 50%;
  box-shadow: 0 0 10px #ddd;
  padding: 10px;
  width: 130px;
  height: 130px;
  img {
    height: 100%;
    width: 100%;
    border-radius: 50%;
    background-color: #eee;
  }
}
.login-form{
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 15px ; 
  box-sizing: border-box
}
.btns{
  text-align: right;
}
</style>
