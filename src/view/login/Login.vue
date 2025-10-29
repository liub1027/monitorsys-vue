<template>
    <div id="login">
        <div>
            <el-form label-width="60px" :rules="rules" :model="loginForm" ref="loginForm" class="loginForm" :label-position="labelPosition">
                <h3 class="loginTitle">监控系统用户登录</h3>
                <el-form-item prop="username" label="账户">
                    <el-input type="text" v-model="loginForm.username" auto-complete="off" placeholder="请输入用户名"></el-input>
                </el-form-item>
                <el-form-item prop="password" label="密码">
                    <el-input type="password" v-model="loginForm.password" @keydown.enter.native="submitForm" auto-complete="off" placeholder="请输入密码"></el-input>
                </el-form-item>
                <el-checkbox v-model="checked" style="margin-bottom: 10px">记住密码</el-checkbox>
                <el-button style="width: 100%" type="primary"  @click="submitForm">登陆</el-button>
                <div class="tip">
                    提示: 为了获得更好的体验建议使用IE 11、谷歌或火狐浏览器进行管理。
                </div>
            </el-form>
        </div>
    </div>
</template>

<script>

    export default {
        name: "Login",
        data() {
            return {
                labelPosition: 'right',
                loginForm: {
                    username: 'admin',
                    password: '123'
                },
                checked:false,
                rules: {
                    username: [{required: true, message: "请输入用户名", trigger: 'blur'}],
                    password: [{required: true, message: "请输入密码", trigger: 'blur'}],
                },
            }
        },
        mounted(){
          // 跳过实际的初始化请求，直接设置默认值
          console.log('跳过初始化请求');
          window.sessionStorage.setItem("init", "initialized");
        },
        methods:{
            submitForm(){

                this.$refs.loginForm.validate((valid) => {
                    if (valid) {
                        // 跳过账户验证，直接模拟登录成功
                        console.log('跳过账户验证，直接登录');
                        
                        // 创建一个模拟的用户响应对象
                        let mockUser = {status: 200, username: this.loginForm.username, message: '登录成功'};
                        
                        // 先将用户信息存储到sessionStorage中
                        window.sessionStorage.setItem("user", JSON.stringify(mockUser));
                        
                        // 模拟初始化配置，确保有init值
                        window.sessionStorage.setItem("init", "initialized");
                        
                        // 显示成功消息
                        this.$message({
                            message: '登录成功，正在跳转...',
                            type: 'success'
                        });
                        
                        // 使用setTimeout确保sessionStorage设置生效后再执行路由跳转
                        setTimeout(() => {
                            this.$router.replace('/home');
                        }, 100);
                    } else {
                        this.$message({
                            message: '请填写必要信息！',
                            type: 'warning'
                        });
                        return false;
                    }
                });
            },
            // 初始化方法已在mounted中重写，不再需要发送实际请求
            init_data(){
                console.log('init_data method called');
            }
        }
    }
</script>

<style>
    #login{
        width: 100%;
        height: 100%;
        background-image: url('./images/login_bg.png') !important;
        background-repeat: no-repeat;
        background-size: 100% 100%;
        background-attachment: fixed;
        background-color: #8ea09f;
        font-size: 12px;
        color: #333;
        overflow:hidden
    }
    .loginForm{
        border-radius: 15px;
        margin: 180px auto;
        width: 400px;
        /*上右下左*/
        padding: 25px 35px 35px 35px ;
        background: #fff;
        border: 1px solid #eaeaea;
        box-shadow: 0 0 25px #eaeaea;
    }
    .loginTitle{
        margin: 15px auto 15px auto;
        text-align: center;
        color: #15a06c;
    }
    .tip{
        margin-top: 10px;
    }
</style>
