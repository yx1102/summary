## 记住密码

![avatar](https://user-gold-cdn.xitu.io/2018/1/30/161462f165cc17dc?w=443&h=335&f=png&s=12535)

**功能**

1.记住密码勾选，点登陆时，将账号和密码保存到cookie，下次登陆自动显示到表单内
2.不勾选，点登陆时候则清空之前保存到cookie的值，下次登陆需要手动输入

> 通过存/取/删cookie实现的；每次进入登录页，先去读取cookie，如果浏览器的cookie中有账号信息，就自动填充到登录框中，存cookie是在登录成功之后，判断当前用户是否勾选了记住密码，如果勾选了，则把账号信息存到cookie当中，效果图如上：



需要安装 `js-base64` 包

```shell
npm install --save js-base64
```

实现代码

```vue
<template>
    <form class="main">
        <!-- 账号 -->
        <div class="item">
            <label for="account">账号</label>
            <input type="text" style="display:none">
            <input type="text" v-model="loginForm.account"  id="account">
        </div>
        <!--密码-->
        <div class="item">
            <label for="password">密码</label>
            <input type="password" style="display:none">
            <input type="password" v-model="loginForm.password" id="password">
        </div>
        <!-- 记住密码 -->
        <div class="item">
            <label>记住密码</label>
            <input type="checkbox" v-model="loginForm.remember">
        </div>
        <!--登录按钮-->
        <button @click="submit">登录</button>
    </form>
</template>

<script>
// 引入base64
import { Base64 } from 'js-base64';
export default {
    data () {
        return {
            // 登陆表单
            loginForm:{
                account:'',
                password:'',
                remember:''
            }
        }
    },
    created () {
        // 在页面加载时从cookie获取登录信息
        let account = this.getCookie("account")
        let password = Base64.decode(this.getCookie("password"))
        // 如果存在赋值给表单，并且将记住密码勾选
        if(userName){
            this.loginForm.account = account
            this.loginForm.password = password
            this.loginForm.remember = true
        }
    },
    methods: {
        // 登录
        submit: function () {
            // 点击登陆向后台提交登陆信息
            axios.post("url",this.loginForm).then(res => {
                 // 储存token（需要封装拦截器，将token放入请求头中）
                this.setCookie('token',res.token)
                // 跳转到首页
                this.$router.push('/Index')
                // 储存登录信息
                this.setUserInfo()
            })
        },
        // 储存表单信息
        setUserInfo: function () {
            // 判断用户是否勾选记住密码，如果勾选，向cookie中储存登录信息，
            // 如果没有勾选，储存的信息为空
            if(this.loginForm.remember){
                this.setCookie("account",this.loginForm.account)
                // base64加密密码
                let passWord = Base64.encode(this.loginForm.password)
                this.setCookie("remember",remember)    
            }else{
                this.setCookie("account","")
                this.setCookie("password","") 
            } 
        },
        // 获取cookie
        getCookie: function (key) {
            if (document.cookie.length > 0) {
            var start = document.cookie.indexOf(key + '=')
            if (start !== -1) {
                start = start + key.length + 1
                var end = document.cookie.indexOf(';', start)
                if (end === -1) end = document.cookie.length
                return unescape(document.cookie.substring(start, end))
            }
            }
            return ''
        },
        // 保存cookie
        setCookie: function (cName, value, expiredays) {
            var exdate = new Date()
            exdate.setDate(exdate.getDate() + expiredays)
            document.cookie = cName + '=' + decodeURIComponent(value) +
            ((expiredays == null) ? '' : ';expires=' + exdate.toGMTString())
        },

    }
}
</script>

<style>
.main {
    width: 300px;
}
.main .item {
    display: flex;
    align-items: center;
    line-height: 30px;
}
.main .item label {
    width: 100px;
}
input:-webkit-autofill {
  	-webkit-box-shadow: 0 0 0px 1000px white inset;  //使用足够大的纯色内阴影覆盖黄色背景
  	border: 1px solid #CCC !important;
}
</style>
```
