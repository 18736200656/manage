<template>
    <div class="login-page">
        <div class="login-area">
            <div class="logo">
                <img src="../../assets/logo.png" alt="">
            </div>
            <div class="form-group">
                <el-form :model="loginForm" :rules="loginRules" ref="loginForm" label-position="left" label-width="0px">
                    <el-form-item prop="phone">
                        <el-input v-model="loginForm.phone" type="text" placeholder="请输入手机号" maxlength="11"></el-input>
                    </el-form-item>
                     <el-form-item prop="captcha" >
                        <el-input v-model="loginForm.captcha" type="text" placeholder="输入图像验证码" style="width:150px;"></el-input>
                        <div @click="refreshCode"><s-identify :identifyCode="identifyCode"></s-identify> </div>
                    </el-form-item>
                    <el-form-item prop="password">
                        <el-input v-model="loginForm.password" type="password" placeholder="密码"></el-input>
                    </el-form-item>
                    <el-button class="btn-login" type="primary" @click="submitForm()">登录</el-button>
                </el-form>
                <div v-if="sys_error" class="err-msg">{{sys_error}}</div>
            </div>
        </div>
    </div>
</template>

<script>
    import SIdentify from '../../components/identify'
    export default {
        name:'login',
        data() {
            return {
                identifyCodes: "1234567890abcdefghijklmnopqrstuvwxyz",
                identifyCode: "1234",
                loginForm: {
                    phone: '',
                    password: '',
                    captcha: ''
                },
                loginRules: {
                    phone: [
                        {required: true, message: '请输入手机号', trigger: 'blur'},
                        {max: 11, message: '请输入11位数字', trigger: 'blur'},
                        {validator: this.ValidatePhone, trigger: 'blur'}
                    ],
                    password :[
                        {required: true, message: '请输入密码', trigger: 'blur'},
                        {min:6,max: 20, message: '请输入6-20位字符', trigger: 'blur'},
                        {validator: this.ValidatePassword, trigger: 'blur'}
                    ],
                    captcha: [
                        {required: true, message: '请输入验证码', trigger: 'blur'},
                        {max: 4, message: '请输入4位数字', trigger: 'blur'},
                        {validator: this.ValidateCaptcha, trigger: 'blur'},
                       
                    ]
                },
                sys_error: '',
                validate: false
            }
        },
        mounted(){
            this.identifyCode = "";
            this.makeCode(this.identifyCodes, 4);
            console.log(this.identifyCodes.length)
        },
        methods: {
            ValidatePhone(rule, value, callback){
                var regExp = /^(13[0-9]|14[579]|15[0-3,5-9]|16[6]|17[0135678]|18[0-9]|19[89])\\d{8}$/;
                if(!regExp.test(value)){
                    callback(new Error('请输入正确的手机号码'))
                }
            },
            ValidateCaptcha(rule, value, callback){
                if(value!=this.identifyCode){
                    callback(new Error('验证码输入错误'))
                }
            },
            ValidatePassword (rule, value, callback) {
                if (value.match(/^\d{6,}$/)) {
                callback(new Error('不能使用纯数字的密码'))
                } else if (/^([a-z0-9\.\@\!\#\$\%\^\&\*\(\)\-\+]){6,20}$/i == false) {
                callback(new Error('请输入6-20位数字、字母和特殊字符（仅限!@#$%^&*()-+）'))
                } else {
                callback()
                }

            },
            submitForm(){
                this.$refs.loginForm.validate((valid) => {
                    if (valid) {
                        let data = {
                            phone: this.loginForm.phone,
                            password: this.loginForm.password
                        }
                        if(this.validate) data.validate = this.loginForm.validate
                        this.loginByEmail(data).then(res => {
                            if(res.data.login){
                                this.$router.push('home')
                            } else{
                                this.sys_error = res.data.message
                                this.validate = res.data.validate
                            }
                        }).catch(err => {
                            this.sys_error = err.response.data
                        })
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            randomNum(min, max) {
                return Math.floor(Math.random() * (max - min) + min);
            },
            refreshCode() {
                console.log(1)
                this.identifyCode = "";
                this.makeCode(this.identifyCodes, 4);
            },
            makeCode(o, l) {
                for (let i = 0; i < l; i++) {
                    this.identifyCode += this.identifyCodes[
                    this.randomNum(0, this.identifyCodes.length)
                    ];
                }
                console.log(this.identifyCode);
            }
        },
        components:{
            SIdentify
        }

    }
</script>

<style lang="scss">
    .login-page{
        width: 100%;
        height: 100%;
        background: #2d3a4b;

        .login-area {
            width: 330px;
            padding-top: 10%;
            margin: 0 auto;
            .logo {
                width: 100%;
                height: 100px;
                text-align: center;
                    img {
                    width: 160px;
                }
            }
        }

        .form-group {
            input::-webkit-input-placeholder{
                color: #7e9fb0;
            }
            position: relative;
            padding-bottom: 20px;
            color: #ccc;
            overflow: hidden;
            .el-input__inner {
                height: 40px;
                line-height: 40px;
                padding-left: 10px;
                background: rgba(0,0,0,.1);
                border: 1px solid rgba(255,255,255,.1);
                border-radius: 4px;
                color: #ccc;
                font-size: 15px;
                overflow: hidden;
                &:hover{
                    border-color: rgba(255,255,255,.2);
                }
                &:focus{
                    border-color: rgba(255,255,255,.4);
                }
            }
            .el-form-item{
                &.is-error .el-input__inner{
                    border-color: #d45b5b;
                }
                &.captcha input{
                    width: 200px;
                }
                &.captcha img{
                    float: right;
                }
            }
            .el-form-item__error{
                color: #d45b5b;
            }
        }

        .btn-login {
            display: block;
            width: 100%;
            height: 40px;
            line-height: 40px;
            margin: 10px 0;
            padding: 0;
            background: #424f60;
            border: none;
            border-radius: 4px;
            color: #ccc !important;
            font-size: 18px;
            text-align: center;
            cursor: pointer;
            &:hover{
                background: darken(#424f60, 2%)
            }
        }
        
        .err-msg{
            color: #d45b5b;
        }
        .tip{
            margin-top: 10px;
            color: #ccc;
        }

        .lang-toggle{
            text-align: center;
            color: #ccc;
            span{
                display: inline-block;
                width: 26px;
                height: 26px;
                line-height: 26px;
                border-radius: 50%;
                color: #ccc;
                font-size: 14px;
                text-align: center;
                cursor: pointer;
                &.cur{
                    background: #ff6363;
                    color: #2d3a4b;
                }
            }
        }

        .footer {
            position: fixed;
            bottom: 0;
            width: 100%;
            height: 80px;
            line-height: 80px;
            color: #ccc;
            font-size: 16px;
            text-align: center;
        }
    }
</style>

