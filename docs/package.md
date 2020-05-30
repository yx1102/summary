# 常用GitHub库

## jsonp跨域异步请求

jsonp(发送jsonp跨域异步请求，返回promise对象)

https://github.com/webmodules/jsonp

>原理：
>
>1). jsonp只能解决GET类型的ajax请求跨域问题
>
>2). jsonp请求不是ajax请求, 而是一般的get请求
>
>3). 基本原理
>
>浏览器端:
>
>- 动态生成 `<script>`来请求后台接口(src就是接口的url)
>
>- 定义好用于接收响应数据的函数(fn), 并将函数名通过请求参数提交给后台(如: callback=fn)
>
>服务器端:
>
>- 接收到请求处理产生结果数据后, 返回一个函数调用的js代码`fn(data)`, 并将结果数据作为实参传入函数调用
>
>浏览器端:
>
>- 收到响应自动执行函数调用的js代码, 也就执行了提前定义好的回调函数, 并得到了需要的结果数据



**下载**

``` js
npm i jsonp
```

**模板**

```js
export const reqWeather = () => {
  // 执行器函数: 内部去执行异步任务, 
  // 成功了调用resolve(), 失败了不调用reject(), 直接提示错误
  return new Promise((resolve, reject) => { 
    jsonp(url, {}, (err, data) => {
      if (!error) { // 成功的
        console.log(data)
        resolve(data)
      } else { // 失败的
        console.log(err)
      }
    })
  }) 
}
```

**例子**

```js
export const reqWeather = (city) => {
  // 执行器函数: 内部去执行异步任务, 
  // 成功了调用resolve(), 失败了不调用reject(), 直接提示错误
  return new Promise((resolve, reject) => { 
    const url = `http://api.map.baidu.com/telematics/v3/weather?location=${city}&output=json&ak=3p49MVra6urFRGOT9s8UBWr2`
    jsonp(url, {}, (error, data) => {
      if (!error && data.error===0) { // 成功的
        const {dayPictureUrl, weather} = data.results[0].weather_data[0]
        resolve({dayPictureUrl, weather})
      } else { // 失败的
        message.error('获取天气信息失败')
      }
    })
  }) 
}
```

**使用**

```js
import './index.js'
// 将状态信息存储到state中
state={
  weather:''
}
// 在合适的时机做一件合适的事情
conponentDidMount(){
  this.getWeather()
}

//获取天气信息显示
getWeather = async () => {
// 发请求
const { weather } = await reqWeather('北京')
// 更新状态
this.setState({
  weather
 })
}
```



## store存储信息

store(存储信息到localstorage)

https://github.com/marcuswestin/store.js 

**下载**

```js
npm i store
```

**模板**

```js
// Store current user
store.set('user', { name:'Marcus' })
// Get current user
store.get('user')
// Remove current user
store.remove('user')
// Clear all keys
store.clearAll()
// Loop over all stored values
store.each(function(value, key) {
console.log(key, '==', value)
})
```

**demo**

```js
import store from 'store'
const USER_KEY = 'user_key'
// 统一暴露
export default {
    // 保存用户信息
    saveUser(user){
        // 使用store库的方法
        store.set(USER_KEY, user)
    },
    // 获取用户信息
    getUser(){
        // 使用store库的方法
       return store.get(USER_KEY) || {}
    },
    // 移除用户信息
    removeUser(){
         // 使用store库的方法
         store.remove(USER_KEY)
    }
}
```

**使用**

```js
import storageUtils from '../../utils/storageUtils'

存储用户：
// 登录成功后将用户的信息存储到localstorage中【此处的user是通过请求获取回来的】
const result = await reqLogin(username,password)
if(result.status===0){
  const user = result.data                    
  storageUtils.saveUser(user)                
}

读取用户信息：
const user = memoryUtils.user
if(user._id){
     return <Redirect to="/admin"/>
}

移除用户信息：
const user = memoryUtils.user
storageUtils.removeUser(user)
```



## vue-thumbnail缩略图

使用这个插件之前要确保已经安装了url-loader file-loader缩略图（小图放大）

https://github.com/LS1231/vue-preview

**下载**

```js
npm i vue-preview -S
```

**配置**

```js
在main.js文件中导入该组件，并挂载到Vue身上
import VuePreview from 'vue-preview';
Vue.use(VuePreview);
```

**使用**

```js
<template>
    <div class="thumbs">
        <vue-preview :slides="thumbsList" class="imgPrev"></vue-preview>
  	</div>
</template>
<script>
export default {
    data() {
        return {
            thumbsList: [],
        };
    },
    methods: {
        getThumbsList(){
            this.$ajax({
                method: "get",
                url: "/thumbs/" ,
            }).then(response => {
                var data = response.data
                if (data.Status == 0) {        
                    data.Data.forEach(item => {
                        item.w = 600;   //设置以大图浏览时的宽度
                        item.h = 400;     //设置以大图浏览时的高度
                        item.src = item.img_url;  //大图
                        item.msrc = item.img_url;  //小图
                    });            
                    this.thumbsList = data.Data
                } else {
                    Toast('获取图片信息失败！');
                }
            });
        },
    },  
};
</script>
<style lang="less" scoped>
  .thumbs {
    /deep/ .my-gallery {
      display: flex;
      flex-wrap: wrap;
      figure {
        width: 30%;
        margin: 5px;
        img {
          width: 100%;
        }
      }
    }
  }
</style>
```



## nuxt操作cookie

https://codesandbox.io/s/github/nuxt/nuxt.js/tree/dev/examples/auth-jwt?from-embed

### nuxt浏览器端操作cookie

js-cookie: https://github.com/js-cookie/js-cookie

**安装**

```js
npm i js-cookie
```

login.vue页面判断客户端服务端是否执行加载cookie

```js
const Cookie = process.client ? require('js-cookie') : undefined
```

store/index.js判断服务端获取cookie

```js
const cookieparser = process.server ? require('cookieparser') : undefined
```

### nuxt服务端解析cookie

```js
npm install cookieparser
```

判断是否需要解析

```js
const cookieparser = process.server ? require('cookieparser') : undefined
```



## Markdown转HTML

### Markdown转HTML标记语言

https://github.com/markdown-it/markdown-it

**下载**

```js
npm install markdown-it --save
```

### Mardown的HTML语法高亮

`注：需要配合markdown-it这个包使用`

https://github.com/highlightjs/highlight.js

```js
npm install highlight.js
```



## vue-validate

**引入**

```js
下载: npm install vee-validate@2.2.9 --save
在项目根目录新建validate.js文件后引入插件:
    import Vue from 'vue'
    import VeeValidate from 'vee-validate'
    
    Vue.use(VeeValidate)
在项目主文件main.js引入validate.js文件
    import './validate'
```

**表单中基本使用**

```js
 <input v-model="email" name="myemail" v-validate="'required'"> // 校验规则
 <span style="color: red;" v-show="errors.has('myemail')">{{ errors.first('myemail') }}</span>  // 样式

 <input v-model="phone" name="phone" v-validate="'required|mobile'"> // 间接定义校验规则mobile
 <span style="color: red;" v-show="errors.has('phone')">{{ errors.first('phone') }}</span>  // 样式  
 
 <input v-model="phone" name="phone" v-validate="{required: true,regex: /^1\d{10}$/}"> // 直接定义校验规则
 <span style="color: red;" v-show="errors.has('phone')">{{ errors.first('phone') }}</span> // 规则出错显示的样式
 
 const success = await this.$validator.validateAll() // 对所有表单项进行验证
 const success = await this.$validator.validateAll(names) // 对指定的所有表单项进行验证,  names 是数组，可以对存放在表单中定义的多个 name 进行部分验证，如果需要使用，可以通过判断当前页面是密码登录还是手机登录等进行部分验证
```

**提示信息本地化**

```js
// 问题: 提示文本默认都是英文的
// errors.first 将错误信息显示出来

import zh_CN from 'vee-validate/dist/locale/zh_CN'
VeeValidate.Validator.localize('zh_CN', {
  messages: zh_CN.messages,
  attributes: {
    phone: '手机号',
    code: '验证码'
  }
})
```

**自定义验证规则**

```js
import VeeValidate from 'vee-validate'
VeeValidate.Validator.extend('mobile', {
  validate: value => {
    return /^1\d{10}$/.test(value)
  },
  getMessage: field => field + '必须是11位手机号码'
})
```



## vue-touch手势事件

https://github.com/vuejs/vue-touch/tree/next

**安装**

```js
npm install vue-touch@next
```

**注册**

```js
var VueTouch = require('vue-touch')
Vue.use(VueTouch, {name: 'v-touch'})
```

**使用**

```vue
<template>
	<v-touch v-on:swipeleft="onSwipeLeft" v-on:tap="onTap">
		<div style="background:red;height:100px;"></div>
	</v-touch>
</template>
<script>
export default(){
    method(){
        onSwipeLeft(){
  			console.log('onSwipeLeft')
		},
		onTap(){
  			console.log('onTap')
		}
    }
}
</script>
```



## px2vm转换像素

https://github.com/evrone/postcss-px-to-viewport/blob/master/README_CN.md

下载

```shell
npm install postcss-px-to-viewport --save-dev
```

在根目录下添加`postcss.config.js`文件,在文件中添加以下内容

> 如果想要保持当前的px结构不转化【例：navbar，tabbar等】，可以是使用的使用添加`ignore`这个类名;
>
> 也可以将不转化的文件名写到`exclude`配置选项中

```js
module.exports = {
  plugins: {
    "postcss-px-to-viewport": {
      viewportWidth: 375, // 视口宽度  对应设计稿
      viewportHeight: 667, // 视口高度 对应设计稿（也可以不配置）
      unitPrecision: 5, // 指定‘px’转换为视窗单位值的最小小数位数（很多时候无法整除）
      viewportUnit: 'vw', // 指定需要转换成的视窗单位，建议使用vw
      selectorBlackList: ['ignore'], // 指定不需要转换的类
      minPixelValue: 1, // 小于或等于1px不转换为视窗单位
      MediaQuery: false, // 允许媒体查询分钟转换px
      // exclude: [/Tabbar\.vue$/] // 内容必须是正则
    }
  }
}
```

