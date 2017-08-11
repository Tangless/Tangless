<template>
    <div id="login-form">
        <div class="login-form">
            <div class="layout" id="layout">
                <!--<a class="ecode" href="javascript:void(0)"></a>-->
                <img class="ecode" src="static/images/ecode.png" />
                <div class="layout-header">
                    <img class="login-img" src="static/images/logo.png" />
                    <h4 class="text-center login-welcome">欢迎登录</h4>
                </div>
                <div class="form-login">
                    <input class="userName form-control" type="text" v-on:input ="inputChange" value="" placeholder="请输入您的用户名" />
                    <input class="userPw form-control" type="password" v-on:input ="inputChange" value="" placeholder="请输入您的密码" />
                    <div class="error-Tip hide">用户名或密码错误</div>
                    <button class="login" v-on:click="login" v-on:keyup.enter="login">立即登录</button>
                </div>
            </div>
        </div>
    </div>
</template>
<style>
    .login-form{
        background-color:#f5f5f5;
        height: 100%;
        height: -webkit-calc(100% - 72px);
        height: -moz-calc(100% - 72px);
        height: calc(100% - 72px);
    }
    .layout {
        background-color: #fff;
        width: 854px;
        height: 620px;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
    }
    .ecode{
        width: 68px;
        height: 83px;
        position: absolute;
        right: 0;
        top: 0;
        opacity: .3;
        /*background: url("/static/images/ecode.gif") no-repeat;*/
    }
    .layout-header{
        height: 192px;
        margin-bottom: 10px;
        padding: 50px 0 22px;
    }
    .login-img{
        width: 49px;
        height: 48px;
        margin: 0 auto;
        display: block;
    }
    .login-welcome{
        font-size: 30px;
        color: #424242;
        font-weight: normal;
        padding-top: 22px;
        padding-bottom: 10px;
    }
    .form-login{
        width: 358px;
        margin: 0 auto;
        padding-bottom: 20px;
    }
    .form-control{
        width: 356px;
        height: 48px;
        line-height: 22px;
        padding: 13px 16px 13px 14px;
        margin-bottom: 14px;
        display: block;
        outline: none;
        border-color: #e0e0e0;
        box-shadow: none;
    }
    .userPw{
        margin-bottom: 28px;
    }
    .error-Tip{
        font-size: 14px;
        height: 21px;
        line-height: 21px;
        color: #ff001f;
        text-align: left;
        margin-bottom: 5px;
    }
    .login{
        width: 100%;
        height: 50px;
        line-height: 50px;
        display: block;
        margin-bottom: 14px;
        text-align: center;
        font-size: 14px;
        color: #fff;
        cursor: pointer;
        background-color: #0D2031;
        outline: none;
        border: none;
        border-radius: 4px;
        margin-top: 10px;
    }
    @media only screen and (max-width: 850px){
        .layout {
            width: 100%;
            width: 400px;
            height: 480px;
        }
        .layout-header{
            height: 127px;
            padding: 25px 0 0;
        }
        .login-welcome{
            font-size: 22px;
            padding-top: 15px;
        }
    }
</style>
<script>
    export default {
        // name: 'my-header',
        data() {
            return {
                author: 'Tangless'
            }
        },
        methods: {
            /*登录提交*/
            login: function () {
                var that = this;
                /*实例化野狗身份认证*/
                var config = {
                    authDomain: "wd1487810104sghfpv.wilddog.com"
                }
                wilddog.initializeApp(config);
                var userName = $(".userName").val();
                var userPW = $(".userPw").val();
                /*已有用户登录*/
                wilddog.auth().signInWithEmailAndPassword(userName, userPW)
                    .then(function () {
                        console.log(wilddog.auth().currentUser)
                        globalData.user.uid = wilddog.auth().currentUser.uid;
                        globalData.user.email = wilddog.auth().currentUser.email;
                        globalData.user.phone = wilddog.auth().currentUser.phone;
                        that.$router.push('/index');
                    }).catch(function (err) {
                        console.info('login failed ->', err);
                        $(".error-Tip").removeClass("hide");
                    });
            },
            /*监听input 内容变化*/
            inputChange:function(){
                $(".error-Tip").addClass("hide");
            }
        }
    }
</script>