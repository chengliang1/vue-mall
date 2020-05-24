<template>
    <div class="login_container">
        <div class="login_box">
            <!--头像区域-->
            <div class="avatar_box">
                <img src="../assets/imgs/logo.jpg" alt="logo">
            </div>
            <!--表单区域-->
            <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" class="login_form">
                <el-form-item prop="username">
                    <el-input prefix-icon="iconfont icon-user" v-model="loginForm.username"></el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input prefix-icon="iconfont icon-3702mima" v-model="loginForm.password" type="password"></el-input>
                </el-form-item>
                <el-form-item class="btn">
                    <el-button type="primary" @click="login">登录</el-button>
                    <el-button type="info" @click="resetLoginForm">重置</el-button>
                </el-form-item>

            </el-form>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Login",
        data(){
            return{
                loginForm:{
                    username: 'admin',
                    password: '123456'
                },
                /*表单验证*/
                loginFormRules: {
                    username: [
                        { required: true, message: '请输入用户名', trigger: 'blur' },
                        { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur'}
                    ],
                    password: [
                        { required: true, message: '请输入密码', trigger: 'blur' },
                        { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur'}
                    ]

                }
            }
        },
        methods: {
            //重置登录表单
            resetLoginForm() {
                this.$refs.loginFormRef.resetFields();

            },
            // 遇到返回值是promise对象时 可添加异步请求 加async 和 await
            login() {
                this.$refs.loginFormRef.validate(async vaild => {
                    if( !vaild) return;
                    const {data : res} =await this.$http.post("login",this.loginForm)
                    if(res.meta.status !== 200) return this.$message.error("登录失败！")
                    this.$message.success("登录成功")
                    window.sessionStorage.setItem("token",res.data.token)
                    this.$router.push("/home")
                });
            }
        }
    }
</script>

<style lang="less" scoped>
    .login_container{
        background-color: #2b4b6b;
        height: 100% ;
     }
    .login_box{
        width: 450px;
        height: 300px;
        background-color: #fff;
        border: 3px;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);

        .avatar_box{
            width: 130px;
            height: 130px;
            border: 1px solid #eee;
            border-radius: 50%;
            padding: 10px;
            box-shadow: 0 0 10px #ddd;
            position: absolute;
            left: 50%;
            transform: translate(-50%,-50%);
            background-color: #fff;


            img{
                width: 100%;
                height: 100%;
                border-radius: 50%;
            }
        }
        .btn{
            display: flex;
            justify-content: flex-end;
        }
        .login_form{
            position: absolute;
            bottom: 0;
            width: 100%;
            padding: 0 20px;
            box-sizing: border-box;
        }
    }


</style>