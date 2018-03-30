<style>

.head-log {
    height: 200px;
}

.company-log {
    width: 300px;
}

.login-wrap {
    position: relative;
    width: 100%;
    height: 100%;
}

.ms-title {
    position: absolute;
    top: 50%;
    width: 100%;
    margin-top: -230px;
    text-align: center;
    font-size: 30px;
    color: #fff;
}

.ms-login {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 300px;
    height: 160px;
    margin: -150px 0 0 -190px;
    padding: 40px;
    border-radius: 5px;
    background: #fff;
}

.login-btn {
    text-align: center;
}

.login-btn button {
    width: 100%;
    height: 36px;
}

.user-defined-style {
    top: 800px;
}

</style>

<template>
<div class="login-wrap">
    <div class="ms-title">xx平台</div>
    <div class="ms-login">
        <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="0px" class="demo-ruleForm">
            <el-form-item prop="username">
                <el-input v-model="ruleForm.username" placeholder="用户名"></el-input>
            </el-form-item>
            <el-form-item prop="password">
                <el-input type="password" placeholder="密码" v-model="ruleForm.password" @keyup.enter.native="submitForm('ruleForm')"></el-input>
            </el-form-item>
            <!-- `checked` 为 true 或 false 
            <el-checkbox v-model="checked">记住密码</el-checkbox>
            <br>
            <br>
            -->
             <div style="padding: 0rem 0 2rem 0;" class="clear">
                    <span class="lf" style="color:#0489cc;">帮助</span>
                 <div class="rt">
                    <el-checkbox v-model="checked" style="color:#a0a0a0;">一周内自动登录</el-checkbox>
                    <span @click="clearCookie" style="cursor: pointer;color: #f19149;font-size: 0.75rem;margin-left: 20px;">忘记密码？</span>
                </div>
            </div>

            <div class="login-btn">
                <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
            </div>
        </el-form>
    </div>
</div>

</template>

<script>

export default {
    data: function() {
        return {
            checked: true,
            ruleForm: {
                username: '',
                password: ''
            },
            userData: [],
            rules: {
                username: [{
                    required: true,
                    message: '请输入用户名',
                    trigger: 'blur'
                }],
                password: [{
                    required: true,
                    message: '请输入密码',
                    trigger: 'blur'
                }]
            }
        }
    },
    //页面加载调用获取cookie值
    mounted() {
        this.getCookie();
    },
    methods: {
        submitForm(formName) {
                const self = this;
                self.$refs[formName].validate((valid) => {
                    if (valid) {

                        //判断复选框是否被勾选 勾选则调用配置cookie方法
                        if (self.checked == true) {
                            console.log("checked == true");
                            //传入账号名，密码，和保存天数3个参数
                            self.setCookie(self.ruleForm.username, self.ruleForm.password, 7);
                        }else {
                          console.log("清空Cookie");
                          //清空Cookie
                          self.clearCookie();
                        }
                        console.log("登陆成功");
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            //设置cookie
            setCookie(c_name, c_pwd, exdays) {
                var exdate = new Date(); //获取时间
                exdate.setTime(exdate.getTime() + 24 * 60 * 60 * 1000 * exdays); //保存的天数
                //字符串拼接cookie
                window.document.cookie = "userName" + "=" + c_name + ";path=/;expires=" + exdate.toGMTString();
                window.document.cookie = "userPwd" + "=" + c_pwd + ";path=/;expires=" + exdate.toGMTString();
            },
            //读取cookie
            getCookie: function() {
                if (document.cookie.length > 0) {
                    var arr = document.cookie.split('; '); //这里显示的格式需要切割一下自己可输出看下
                    for (var i = 0; i < arr.length; i++) {
                        var arr2 = arr[i].split('='); //再次切割
                        //判断查找相对应的值
                        if (arr2[0] == 'userName') {
                            this.ruleForm.username = arr2[1]; //保存到保存数据的地方
                        } else if (arr2[0] == 'userPwd') {
                            this.ruleForm.password = arr2[1];
                        }
                    }
                }
            },
            //清除cookie
            clearCookie: function() {
                this.setCookie("", "", -1); //修改2值都为空，天数为负1天就好了
            }
    }
}

</script>
