## 登录后跳转回原界面

route

```js
router.beforeEach((to, from, next) => {
  if(from.name == "Login"){ // 如果不需要权限校验，直接进入路由界面
    next();
  }else if(to.meta.requireAuth){
    // 判断该路由是否需要登录权限
    if (localStorage.getItem("token")) {  // 获取当前的token是否存在
      next();
    } else {
      alert('请登录后再访问')
      next({
        path: '/login', // 将跳转的路由path作为参数，登录成功后跳转到该路由
        query: {redirect: to.fullPath}
      })
    }
  }else { // 如果不需要权限校验，直接进入路由界面
    next();
  }
})
```



login

```js
// 	请求结束后，判断地址栏是否有跳转地址，如果有跳转到对应页面，没有则跳转至首页
if (this.$route.query.redirect) {
  //如果存在参数
  let redirect = this.$route.query.redirect;
  this.$router.replace(redirect); //则跳转至进入登录页前的路由
} else {
  this.$router.replace("/"); //否则跳转至首页
}
```



## 401错误

需结合 `router.beforeEach` 使用

```js
axios.interceptors.request.use(config => {
  // 将json格式请求体转换为urlencode
  const { method, data } = config
  if (method.toLowerCase() === 'post' && data instanceof Object) {
    // 此处要转成urlencode模式，不是转成对象模式
    config.data = qs.stringify(data)
  }

  // 判断页面是否需要携带token才能请求
  if (config.headers.needToken) {
    const token = JSON.parse(localStorage.getItem('token_key'))
    if (token) {
      config.headers.Authorization = token
    } 
  }
  return config;
});


// Add a response interceptor
axios.interceptors.response.use(response => {

  //直接返回，作为下一个成功结果的回调
  return response.data

}, error => {
  // 判断是什么错误
  const { response, message} = error
  // 1. token超时
  if (response.status === 401) {
    // 调用退出登录的方法
    store.dispatch('resetUser')
    if (router.currentRoute.path !== '/login') {
      Toast(message)
      router.push('/login')
    }
  }

  // 2. 一般请求错误
  else {
    Toast('请求错误' + message)
  }

  return new Promise(() => { })
});

 
axios.interceptors.response.use(function (response) {
    return response;
  }, function (error) {
     if(error.response.status == 401 || error.response.status == 402){
         router.push('/login')
         Vue.prototype.$msg.fail(error.response.data.message)
     }
    return Promise.reject(error);
  });
```



## router页面使用封装的组件

router.js

```js
import { Toast } from 'mint-ui
Toast(message)
```



或

main.js

```js
import vant from 'vant'
import {Toast} from 'vant'
import 'vant/lib/index.css';
Vue.prototype.$msg = Toast
```

router.js

```js
Vue.prototype.$msg.fail(error.response.data.message)
```





## 记住密码

![1590388030771](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1590388030771.png)

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



## tabbar封装

```vue
<template>
  <footer class="footer_guide">
    <span class="guide_item" :class="{on: $route.path==='/video'}" @click="goTo('/video')">
      <span class="item_icon">
        <i class="fas fa-video"></i>
      </span>
      <span>视频</span>
    </span>
    <span class="guide_item" :class="{on: $route.path==='/music'}" @click="goTo('/music')">
      <span class="item_icon">
        <i class="fas fa-music"></i>
      </span>
      <span>音乐</span>
    </span>
    <span class="guide_item" :class="{on: $route.path==='/profile'}" @click="goTo('/profile')">
      <span class="item_icon">
        <i class="fas fa-user"></i>
      </span>
      <span>我的</span>
    </span>
  </footer>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {
    goTo(path) {
      // 编程式路由导航
      this.$router.replace(path);
    }
  }
};
</script>
<style lang='less' scoped>
.footer_guide {
  border-top: 1px solid #ccc;
  position: fixed;
  z-index: 100;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #fff;
  width: 100%;
  height: 50px;
  display: flex;
  .guide_item {
    display: flex;
    flex: 1;
    text-align: center;
    flex-direction: column;
    align-items: center;
    margin: 5px;
    color: #999999;
    &.on {
      color: #02a774;
    }
    span {
      font-size: 12px;
      margin-top: 2px;
      margin-bottom: 2px;
      .iconfont {
        font-size: 22px;
      }
    }
  }
}
</style>
```



## navbar封装

```vue
<template>
  <div class="navbar">
    <div class="left">
      <slot name="left"></slot>
    </div>
    <div class="center">
      <slot name="center"></slot>
    </div>
    <div class="right">
      <slot name="right"></slot>
    </div>
  </div>
</template>
<script>
export default {};
</script>
<style lang="less" scoped>
.navbar {
  display: flex;
  height: 44px;
  line-height: 44px;

  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 999;

  text-align: center;

  .left,
  .right {
    width: 60px;
  }
  .center {
    flex: 1;
  }
}
</style>
```



## Vue-Toast插件封装

toast文件夹的`Toast.vue`页面

```vue
<template>
  <div id="toast">
    <transition name="toast">
      <div class="toast_div" v-show="isShowTip">{{ message }}</div>
    </transition>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isShowTip: false,
      message: ""
    };
  },
  methods: {
    show(msg, delay = 1000) {
      this.isShowTip = true;
      this.message = msg;
      setTimeout(() => {
        this.isShowTip = false;
      }, delay);
    }
  }
};
</script>
<style scoped>
/**
* 给加入购物车做样式即过渡
*/
.toast_div {
  font-size: 0.8rem;
  text-align: center;
  line-height: 2rem;
  height: 2rem;
  width: 10rem;
  background-color: rgba(29, 24, 24, 0.9);
  border-radius: 0.4rem;
  border: 0.04rem solid rgba(0, 0, 0, 0.4);
  color: white;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.toast-enter,
.toast-leave-to {
  opacity: 0;
}
.toast-enter-active {
  transition: opacity 1s;
}
.toast-leave-active {
  transition: opacity 1s;
}
</style>
```

toast文件夹`index.js`页面

```js
const obj = {};

import Toast from "./Toast.vue";

obj.install = function(Vue) {
  //创建组件构造器
  const toastConstructor = Vue.extend(Toast);

  //通过构造器创建组件实例
  const toast = new toastConstructor();

  //将实例挂载到元素上，并添加到DOM中
  toast.$mount(document.createElement("div"));

  document.body.appendChild(toast.$el);

  //给Vue原型添加上挂载后的实例
  Vue.prototype.$toast = toast;
};

export default obj;
```

`main.js`文件夹引入注册

```js
import Toast from "components/common/toast/index.js";
Vue.use(Toast)
```

组件中使用

```js
this.$toast.show("加入购物车");
```



## tabs切换

tabs标签横向或不用随着滚动条的高度变化而变化

```js
<li v-for="(item, index) in menu" :key="index" :class="index === currentIndex ? 'current' : ''">

export default {
  data() {
    return {
      currentIndex: 0
    };
  },
  methods: {
    changeIndex(index){
      this.currentIndex = index
      this.$emit('changeTab', index)
    }
  },
};
```

tabs标签纵向并且需要随着高度的变化而切换

```js
<li v-for="(item, index) in menu" :key="index" :class="index === currentIndex ? 'current' : ''">

computed: {
    // 当前的选中的li
    currentIndex(){
      const index = this.tops.findIndex((item,index) => this.scrollY>=item && this.scrollY < this.tops[index+1])
      // 右侧滑动，左侧的样式还会发生改变，显示根据下标的改变来知道样式的改变
      if(index !== this.index && this.leftScroll){
        this.index = index
        // 左侧列表选中项滑到顶部
        const li = this.$refs.leftUl.children[this.index]
        this.leftScroll.scrollToElement(li, 500)
      }
      return index
    }
  },
```







## 封装better-scroll的使用

better-scroll的模板

```vue
<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>
<script>
import BScroll from "better-scroll";
export default {
  data() {
    return {
      scroll: null
    };
  },
  props: {
    probeType: {
      type: Number,
      default: 0
    },
    pullUpLoad: {
      type: Boolean,
      default: false
    },
    pullDownRefresh: {
      type: Boolean,
      default: false
    },
    scrollX:{
      type: Boolean,
      default: false 
    }
  },
  methods: {
    _initScroll() {
      if (!this.scroll) {
        this.scroll = new BScroll(this.$refs.wrapper, {
          scrollX: this.scrollX
          click: true,
          mouseWheel: true,
          probeType: this.probeType,
          pullUpLoad: this.pullUpLoad,
          pullDownRefresh: this.pullDownRefresh
        });

        if (this.probeType !== 0) {
          // 监测滚动位置
          this.scroll.on("scroll", position => {
            this.$emit("scrollPosition", position);
          });

          // 监测滚动条最末端的位置
          this.scroll.on("scrollEnd", position => {
            this.$emit("scrollEndPosition", position);
          });
        }

        if (this.pullUpLoad) {
          this.scroll.on("pullingUp", () => {
            this.scroll.finishPullUp()
            this.$emit("pullingUp");
          });
        },
            
        if (this.pullDownRefresh) {
          this.scroll.on("pullingDown", () => {
            this.$emit("pullingDown");
          });
        },   
      }
    },

    // 到达指定的位置
    scrollTo(x, y, time = 300) {
      this.scroll && this.scroll.scrollTo(x, y, time);
    },
      
    // 到达指定的元素
    scrollToElement(el,time = 300) {
      this.scroll && this.scroll.scrollToElement(el, time);
    },

    // 刷新
    refresh() {
      this.scroll && this.scroll.refresh();
    },

    // 重复加载
    finishPullUp() {
      this.scroll && this.scroll.finishPullUp();
    }
  },
  mounted() {
    this.$nextTick(() => {
      this._initScroll()
    })
  }
};
</script>
```

组件中使用

> 引入注册组件，给scroll设置需要滚动的范围
>
> 将需要滚动的内容放到`scroll`中包裹起来
>
> 如果要开启横向滚动，父组件传值给子组件`:probeType="3"`
>
> 如果要监听滚动距离，父组件传值给子组件`:scrollX="true"`
>
> 如果要上拉加载，父组件传值给子组件`:pullUpLoad="true"`并监测滚动位置事件`@scrollPosition="scrollPosition"`和上拉加载事件`@pullingUp="loadMore"`
>
> 如果下拉刷新，父组件传值给子组件`:pullDownRefresh="true"`并监听下拉刷新事件`@pullingDown="pullingDown"`
>
> 如果直接达到某一个地方，父组件调用子组件方法`this.$refs.scroll.scrollTo(0, this.scrollY, 0)`,点击达到指定位置或进入当前页面回到对应的位置
>
> 如果到达指定的元素位置，父组件调用子组件方法`this.$refs.scroll.scrollToElement(li, 500)`,
>
> 如果重置滚动条高度，父组件调用子组件方法`this.$refs.scroll && this.$refs.scroll.refresh()`,一般用于mounted挂载时重置，或等图片加载完成后重置

```vue
<template>
<scroll class="homeScroll" ref="homeScroll" :probeType="3" :pullUpLoad="true" @scrollPosition="scrollPosition" @pullingUp="loadMore">
  <!-- 轮播图 -->
  <home-swipe :banner="banner" class="home_swiper"></home-swipe>
  <!-- 推荐列表 -->
  <home-recommend :recommend="recommend"></home-recommend>
</scroll>
</template>

<script>
methods:{
    scrollPosition(position) {
      this.scrollY = Math.abs(position.y);
    },

    loadMore() {
      this.getGoodsList(this.currentType);
    },
    
    // 请求的下拉页面事件    
    async getGoodsList(type) {
      // 在不同的类型上页码值随着滑动而增加
      const page = this.goods[type].page + 1;
      const res = await reqHomeGoodsList(type, page);

      if (res.success) {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;
      }
    }
}
</script>


<style scoped lang="less">
    .homeScroll{
        height: calc(100% - 93px);  /*calc两边需要有空格，否则实现不了*/
    	overflow: hidden;
    }
</style>
```



### 图片导致的滚动bug

**请求的数据中图片没加载完成不能形成滚动**

解决办法：监听每一张图片是否加载完成, 只要有一张图片加载完成了, 执行一次refresh()

**监听图片加载完成**

```html
原生的js监听图片: img.onload = function() {}
<img onload=function() {}>
Vue中监听: @load='方法'
<img @load="loadImg">

<img :src="goodItem.img" v-if="goodItem.img" @load="imageLoad"/>

监听加载完成后刷新滚动条的高度
loadImg: debounce(function(){{
   this.$refs.rightScroll && this.$refs.rightScroll.refresh()
},
```

### 获取offsetTop不正确的bug

原因：图片没有加载完成就获取offsetTop，导致不正确

解决办法：待图片加载完成再获取

```html
<img :src="goodItem.img" v-if="goodItem.img" @load="imageLoad"/>

监听加载完成后刷新滚动条的高度
loadImg: debounce(function(){{
   this.tabHeight = this.$refs.tabcontrol.$el.offsetTop
},
```

### 导航条吸顶

导航条滚动到一定距离之后吸顶,在scroll滚动外侧放置一个隐藏的tab栏，到达指定位置后tab栏显示，原来的tab栏隐藏

```vue
<template>
	<!-- 到达指定位置出现的tab栏 -->
    <tab-control v-show="isShowTab"></tab-control>

    <scroll>
      <!-- 轮播图 -->
      <home-swipe></home-swipe>

     <div>
        <!-- tab栏 -->
        <tab-control v-show="!isShowTab"></tab-control>
      </div>
    </scroll>
</template>
<script>
computed:{
    isShowTab() {
      if (this.scrollY > this.tabHeight) {
        return true;
      } else {
        return false;
      }
    },
}
</script>
```





## 上拉刷新下拉加载

### 上拉加载

滚动条触底 开始加载下一页数据

1. 找到滚动条触底事件

2. 判断还有没有下一页数据

3. 获取到总页数  计算总条数

   ​      总页数 = Math.ceil(总条数 /  页容量  pagesize)【总页数  = Math.ceil( 23 / 10 ) = 3】

4. 获取到当前的页码  pagenum

   ​    判断一下 当前的页码是否大于等于 总页数 【if(totalPage>=this.pagenum)】

   ​    表示 没有下一页数据

5. 假如没有下一页数据 弹出一个提示

6. 假如还有下一页数据 来加载下一页数据
       1 当前的页码 ++

   ​    2 重新发送请求  this.getGoodsList()

   ​    3 数据请求回来  要对data中的数组 进行 拼接 而不是全部替换！！！

   ​      【goodsList:[...this.data.goodsList,...res.goods] 	或	 this.list.**push**(...res.data.list)】

### 下拉刷新页面

1. 触发下拉刷新事件
2. 重置 数据 数组    【goodsList: []】
3. 重置页码 设置为1
4. 重新发送请求 this.getGoodsList()
5. 数据请求回来 需要手动的关闭 等待效果【在请求成功的函数写关闭代码】



## 加入购物车

1. 先绑定点击事件
2. 获取 缓存/vuex 中的购物车数据 数组格式
3. 先判断【findIndex】当前的商品是否已经存在于 购物车 
4. 已经存在 修改商品数据  执行购物车数量++ 重新把购物车数组 填充回缓存中
5. 不存在于购物车的数组中 直接给购物车数组添加一个新元素push 新元素 带上 购买数量属性 num  重新把购物车数组 填充回缓存中
6. 弹出提示添加到购物车成功

### js的方法

```js
handleCartAdd() {
  // 1 获取缓存中的购物车 数组
  let cart = JSON.parse(localStorage.getItem('cart') || '[]')
  // 2 判断 商品对象是否存在于购物车数组中
  let index = cart.findIndex(v => v.goods_id === this.GoodsInfo.goods_id);
  if (index === -1) {
    //3  不存在 第一次添加
    this.GoodsInfo.num = 1;
    this.GoodsInfo.checked = true;
    this.cartList.push(this.GoodsInfo);
  } else {
    // 4 已经存在购物车数据 执行 num++
    this.cartList[index].num++;
  }
  // 5 把购物车重新添加回缓存中
  localStorage.setItem("cart", JSON.stringify(cart));
},
```

### vuex的方法1

```js
actions: {
 addToCart({commit, state}, good){
    const index = state.cartList.findIndex(item => item.iid === good.iid)
    if(index === -1){
      commit(ADD_CART, good)
    }else{
      commit(ADD_COUNT, index)
    }
  },
}
mutations: {
  [ADD_CART](state, good){
    Vue.set(good, "count", 1)
    state.cartList.push(good)
  },

  [ADD_COUNT](state, index){
    state.cartList[index].count ++
  },
} 
```



### vuex的方法2

```js
actions: {
 addToCart({commit, state}, good){
  let oldProduct = null
  state.cartList.some(item => {
    if(item.iid === good.iid){
      oldProduct = item
    }
  })
  if(oldProduct){
    commit(ADD_COUNT, good)
  }else{
    commit(ADD_CART, good)
  }
}
mutations: {
  [ADD_CART](state, good){
    Vue.set(good, "count", 1)
    state.cartList.push(good)
  },

  [ADD_COUNT](state, good){
    good.count ++
  },
} 
```

## 商品收藏

页面`created`时，读取缓存中的商品收藏的数据

1. 判断当前商品是不是被收藏 
2. 是 ------------>   改变页面的图标
3. 不是
   1. 点击商品收藏按钮 
   2. 判断该商品是否存在于缓存数组中 【some】 请求完成才可以判断是否有该商品
   3. 已经存在 把该商品删除 【splice】
   4. 没有存在 把商品添加到收藏数组中 存入到缓存中即可 【push】

### 方式一

优点：不需要单独用一个数组来保存是否被收藏

在获取到商品列表数据后，给每个商品添加一个标识`isCollect:false`后再将内容保存会data中，通过读取数据来确定是否有收藏商品；

如果有收藏`:class="{active : item.isCollect}"`



### 方式二

获取当前商品的id，默认是给商品加收藏

```js
<div @click="handelCollect">收藏</div>

data: {
  // 当前商品是否被收藏
  isCollect:false,
  collectList:[]
},
mounted(){
  async getGoodsDetail(){
    const result = await request({url: '/goods/detail', data:this.queryInfo})
    this.goodsObj = result

    // 加载缓存中的商品收藏的数据
    this.collectList = JSON.parse(localStorage.getItem("collectList") || '[]')
    // 判断缓存中是否有该商品
    this.isCollect = collectList.some(item => item.goods_id === this.goodsObj.goods_id)
   },
}

methods:{
  // 商品收藏
  handelCollect(){
    // 1 判断该商品是否存在于缓存数组中
    const index = this.collectList.findIndex(item => item.goods_id === this.goodsObj.goods_id)
    
    // 2 已经存在 把该商品删除
    if(index !== -1){
      this.collectList.splice(index, 1)
      this.isCollect = false
    }else{
      // 3 没有存在 把商品添加到收藏数组中 存入到缓存中即可
      this.collectList.push(this.goodsObj)
      this.isCollect = true
    }

    // 4. 将值存入缓存中
    localStorage.setItem("collectList", JSON.stringify(this.collectList))
  }
}
```



## 商品支付

支付按钮

先获取token然后，判断缓存中有没有token

没token 跳转到登录页面登录

有token

1. 准备发送请求 创建订单 获取订单编号
2. 发起 预支付接口
3. 发起微信支付   wx-requestPayment
4. 查询后台 订单状态 分别提示支付成功和支付失败
5. **手动删除缓存中 已经被选中了的商品**
    let newCart=wx.getStorageSync("cart")
    newCart=newCart.filter(v=>!v.checked) // 过滤只剩下没有被选中的商品
6. 删除后的购物车数据 填充回缓存
    wx.setStorageSync("cart", newCart)
7. 再跳转页面



## 多层嵌套评论

comment_id 为当前的评论评级

如果 parent_id 有值则为回复的评论

https://www.bilibili.com/video/BV1vT4y137So?p=33

```json
[
{ comment_id: 1, user_id: 43, comment_date: "04-23", comment_content: "蜡笔小新很好看!", parent_id: null },
        { comment_id: 2, user_id: 19, comment_date: "04-24", comment_content: "还不错哦!很好看", parent_id: null },
        { comment_id: 3, user_id: 17, comment_date: "04-25", comment_content: "我也感觉蜡笔小新很好看", parent_id: "1" },
        { comment_id: 4, user_id: 14, comment_date: "04-26", comment_content: "我感觉机器猫更好看一点", parent_id: "3" },
        { comment_id: 5, user_id: 13, comment_date: "04-27", comment_content: "好看,已三连!", parent_id: null },
        { comment_id: 6, user_id: 21, comment_date: "04-26", comment_content: "你是机器猫的粉丝吗", parent_id: "4" },
        { comment_id: 7, user_id: 14, comment_date: "04-27", comment_content: "是的,我是机器猫的粉丝", parent_id: "6" },
        { comment_id: 8, user_id: 23, comment_date: "04-27", comment_content: "我更喜欢白嫖!", parent_id: "5" },
        { comment_id: 9, user_id: 25, comment_date: "04-28", comment_content: "你个白嫖怪", parent_id: "8" }
]
```



## 歌词解析

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    p.active {
      color: red;
    }
  </style>
</head>
<!-- https://www.bilibili.com/video/BV1ye411p7w3?p=48 -->
<body>
  <div id="app">
    <audio src="./mp3/2.mp3" controls ref="audio"></audio>
    <div v-for="(value, key, index) in lrcObj" :key="key" style="text-align: center;">
      <!-- 显示时：currentTime 大于当前的key， 小于下一个key ==> 设计一个key数组 -->
      <p :class="currentTime >= currentKeys[index] &&  currentTime < currentKeys[index + 1] ? 'active' : ''">{{value}}</p>
    </div>
  </div>
  <script src="./js/vue.js"></script>
  <script>
    new Vue({
      el: '#app',
      data: {
        lrc: {
          "version": 1,
          "lyric": "[by:葫芦-小-金刚]\n[00:00.000] 作曲 : 郑秋枫\n[00:01.000] 作词 : 瞿琮\n[00:07.754] 百灵鸟从蓝天飞过\n[00:25.848] 我爱你中国\n[00:44.110]\n[01:09.562] 我爱你中国\n[01:16.698] 我爱你中国\n[01:23.330] 我爱你春天蓬勃的秧苗\n[01:29.878] 我爱你秋日金黄的硕果\n[01:36.927] 我爱你青松气质\n[01:43.418] 我爱你红梅品格\n[01:50.103] 我爱你家乡的甜蔗\n[01:54.920] 好像乳汁滋润着我的心窝\n[02:04.858] 我爱你中国\n[02:12.027] 我爱你中国\n[02:19.002] 我要把最美的歌儿献给你\n[02:25.517] 我的母亲我的祖国\n[02:34.071]\n[02:46.141] 我爱你中国\n[02:52.828] 我爱你中国\n[02:59.436] 我爱你碧波滚滚的南海\n[03:05.749] 我爱你白雪飘飘的北国\n[03:13.225] 我爱你森林无边\n[03:19.760] 我爱你群山巍峨\n[03:26.110] 我爱你淙淙的小河\n[03:31.159] 荡着清波从我的梦中流过\n[03:41.374] 我爱你中国\n[03:48.376] 我爱你中国\n[03:55.123] 我要把美好的青春献给你\n[04:01.591] 我的母亲我的祖国\n[04:09.380] 啊~~~\n[04:22.596] 我要把美好的青春献给你\n[04:29.422] 我的母亲我的祖国\n"
        },
        lrcObj: {},
        currentTime:0,  // 当前播放的时间
        duration: 0,   // 总时长
        currentKeys:[]
      },
      mounted() {
        this.formateLrc()
      },
      methods: {
        // 将歌词格式化为数组对象[{1:"jfkd"},{34:"jfklds"}]
        formateLrc() {
          // 去除所有的空格,"[00:12.570]难以忘记初次见你"
          const arr = this.lrc.lyric.split("\n")
          // 分开数字和歌词的正则表达
          const reg = /\[\d*:\d*(\.|:)\d*]/g
          let obj = {}
          for (let i = 0; i < arr.length; i++) {
            // 歌词时间
            const time = arr[i].match(reg)
            // 歌词文本
            const text = arr[i].replace(time, "")

            // 含有所有时间的字符串
            if (time) {
              // 将时分秒转成数字格式
              const min = Number(time[0].match(/\[\d*/i).toString().slice(1)) //"[00" => 00
              const second = Number(time[0].match(/:\d*/i).toString().slice(1)) //":34" =>34
              const currentTime = min * 60 + second
              obj[currentTime] = text
              this.currentKeys.push(currentTime)
            }
          }
          this.lrcObj = obj

          this.$nextTick(() => {
            this.addEventAudio()
          })
        },

        // 监听播放器的事件
        addEventAudio(){
          // 1. 歌曲当前播放的时间
          this.$refs.audio.addEventListener('timeupdate',() => {
            this.currentTime = this.$refs.audio.currentTime
          })
          // 2. 歌曲的总时长
          this.$refs.audio.addEventListener('canplay',() => {
            this.duration = this.$refs.audio.duration
          })
        }
      }
    })
  </script>
</body>
</html>
```



## 城市列表

```html
<!DOCTYPE html>
<html lang="en">
<!-- https://www.bilibili.com/video/BV1KE411s7kX?p=100 -->
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <div v-for="item in cities" :key="item.cityId">
      <p style="background-color: aquamarine;">{{item.letter}}</p>
      <p v-for="(city, index) in item.list" :key="index">{{city.name}}</p>
    </div>
  </div>
  <script src="./js/vue.js"></script>
  <script>
    var vm = new Vue({
      el: '#app',
      data: {
        cityData: [
          {
            "cityId": 110100,
            "name": "北京",
            "pinyin": "beijing",
            "isHot": 1
          },
          {
            "cityId": 650500,
            "name": "哈密",
            "pinyin": "hami",
            "isHot": 0
          },
          {
            "cityId": 320200,
            "name": "无锡",
            "pinyin": "wuxi",
            "isHot": 0
          }
        ],
        cities:[]  
      },
      created() {
        this.formatCity()
      },
      methods: {
        formatCity(){
          let letterArr = []
          // 选出26个字母
          for(let i = 65; i < 91; i++){
            letterArr.push(String.fromCharCode(i))
          }
          let newCityList = []
          // 从数组中取出数据进行对比
          for(let j = 0; j <letterArr.length; j++){
            // 过滤字母相同的数组
            const tempArr = this.cityData.filter(item => item.pinyin.slice(0,1) === letterArr[j].toLowerCase())
            if(tempArr.length > 0){
              newCityList.push({
                "letter": letterArr[j],
                "list":tempArr
              })
            }
          }
          this.cities = newCityList
        }
      },
    })
  </script>
</body>
</html>
```

