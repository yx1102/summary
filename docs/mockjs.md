# mockjs

案例：运用element-ui实现含分页数据的请求；增加；删除等
![](https://img2020.cnblogs.com/blog/1983442/202005/1983442-20200517231209541-378870749.png)

mockjs文件夹的index.js
```
import Mock from 'mockjs'

const data = Mock.mock({
  "list|20-60": [
    {
      "id": '@increment(1)',
      "title": "@ctitle",
      "content": "@cparagraph",
      "add_time": "@date(yyyy-MM-dd hh:mm:ss)"
    }
  ]
})

// 删除
Mock.mock('/api/delete/news', 'post', (options) => {
  let body = JSON.parse(options.body)
  const index = data.list.findIndex(item => item.id === body.id)
  data.list.splice(index, 1)
  return {
    status: 200,
    message: '删除成功',
    list: data.list
  }
})

// 添加
Mock.mock('/api/add/news', 'post', (options) => {
  let body = JSON.parse(options.body)
  
  data.list.push(Mock.mock({
    "id": '@increment(1)',
    "title": body.title,
    "content": body.content,
    "add_time": "@date(yyyy-MM-dd hh:mm:ss)"
  }))

  return {
    status: 200,
    message: '添加成功',
    list: data.list
  }
})


// 含有分页的数据列表,有需要接受的参数要使用正则匹配
// /api/get/news?pagenum=1&pagesize=10
Mock.mock(/\/api\/get\/news/, 'get', (options) => {
  console.log(options)
  // 获取传递的参数pageindex
  const pagenum = getQuery(options.url,'pagenum')
  // 获取传递的参数pagesize
  const pagesize = getQuery(options.url,'pagesize')
  // 截取数据的起始位置
  const start = (pagenum-1)*pagesize
  // 截取数据的终点位置
  const end = pagenum*pagesize
  // 计算总页数
  const totalPage = Math.ceil(data.list.length/pagesize)
  // 数据的起始位置：(pageindex-1)*pagesize  数据的结束位置：pageindex*pagesize
  const list = pagenum>totalPage?[]:data.list.slice(start,end)

  return {
    status: 200,
    message: '获取新闻列表成功',
    list: list,
    total: data.list.length
  }
})

const getQuery = (url,name)=>{
  const index = url.indexOf('?')
  if(index !== -1) {
    const queryStrArr = url.substr(index+1).split('&')
    for(var i=0;i<queryStrArr.length;i++) {
      const itemArr = queryStrArr[i].split('=')
      console.log(itemArr)
      if(itemArr[0] === name) {
        return itemArr[1]
      }
    }
  }
  return null
}
```

App.vue
```
<template>
  <div id="app">
    <!-- 头部 -->
    <h3 style="text-align: center">数据展示</h3>

    <!-- 添加 -->
    <el-row :gutter="20">
      <el-col :span="8"
        ><el-input v-model="title" placeholder="请输入标题"></el-input
      ></el-col>
      <el-col :span="8"
        ><el-input v-model="content" placeholder="请输入内容"></el-input
      ></el-col>
      <el-col :span="8"
        ><el-button type="primary" @click="handelAdd">添加</el-button></el-col
      >
    </el-row>

    <!-- 表格 -->
    <template>
      <el-table :data="tableData" style="width: 100%">
        <!-- <el-table-column label="图片" width="120">
          <template v-if="tableData[0]"
            ><img :src="tableData[0].img_url" width="100" height="100"
          /></template>
        </el-table-column> -->
        <el-table-column prop="title" label="标题" width="160">
        </el-table-column>
        <el-table-column prop="content" label="内容"> </el-table-column>
        <el-table-column prop="add_time" label="时间" width="160">
        </el-table-column>
        <el-table-column label="操作" width="160">
          <template slot-scope="scope">
            <el-button @click="handleDelete(scope.row.id)" type="danger"
              >删除</el-button
            >
          </template>
        </el-table-column>
      </el-table>
    </template>

    <el-button type="primary" @click="prev">上一页</el-button>
    <el-button type="primary" @click="next">下一页</el-button>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      tableData: [],
      title: "",
      content: "",
      pagenum: 1,
      pagesize: 10,
      total: 0
    };
  },
  methods: {
    // 获取数据列表
    async getList() {
      // axios.get("/api/get/news").then(res => (this.tableData = res.data.list));
      const result = await axios.get("/api/get/news",{params: {pagenum: this.pagenum, pagesize: this.pagesize}})
      this.tableData = result.data.list
      this.total = result.data.total
    },

    // 删除
    async handleDelete(id) {
      const result = await axios.post("/api/delete/news", { id });
      this.getList();
    },

    // 添加数据
    async handelAdd() {
      if (!this.title || !this.content) {
        alert("请输入内容");
        return;
      }

      const result = await axios.post("/api/add/news", {
        title: this.title,
        content: this.content
      });
      this.getList();
      this.title = "";
      this.content = "";
    },

    prev(){
      if(this.pagenum > 1){
        this.pagenum --
      this.getList()
      }
    },

    next(){
      if(this.pagenum * this.pagesize < this.total ){
        this.pagenum ++
        this.getList()
      }else{
        alert('没有更多的数据')
      }
    }
  },
  created() {
    this.getList();
  }
};
</script>
```

main.js
```
import './mockjs/index'
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);
```

## 概念

### 什么是mockJs

生成随机数据，拦截 Ajax 请求 

参考文档：http://mockjs.com/examples.html


### 为什么使用mockJs

如果后端接口还未开发完成，我们自己手动模拟后端接口返回随机数据完成交互功能

1. 采用json数据模拟，生成数据比较繁琐，也比较有局限性，没办法达到增删改查
2. 采用mockJs进行模拟数据，可以模拟各种场景（get、post）生成接口，并且随机生成所需数据，还可以对数据进行增删改查 


### 使用mockJs

通过vue-cli创建基本项目

- 在项目中安装mock

  ```js
  npm install mockjs
  ```

- 在项目中新建mock文件夹，在mock文件夹中创建index.js

  ```js
  //引入mock模块
  import Mock from 'mockjs';
  ```

- 将mock文件夹的index.js文件在main.js中导入

  ```js
  import Vue from 'vue'
  import App from './App.vue'
  import './mock/index.js'
  
  
  new Vue({
    render: h => h(App),
  }).$mount('#app')
  ```

## mock语法

### 生成字符串

- 生成指定次数字符串

  ```js
  const data = Mock.mock({
      "string|4": "yx！"
  })
  console.log(data)
  ```

- 生成指定范围长度字符串

  ```js
  const data = Mock.mock({
      "string|1-8": "yx"
  })
  console.log(data)
  ```



### 生成文本

- 生成一个随机字符串

  ```js
  const data = Mock.mock({
      "string": "@cword"
  })
  console.log(data)
  ```

- 生成指定长度和范围

  ```js
  const data = Mock.mock({
      string: "@cword(1)",
      str: '@cword(10,15)'
  })
  console.log(data)
  ```

### 生成标题和句子

- 生成标题和句子

  ```js
  const data = Mock.mock({
      title: "@ctitle",
      sentence: '@csentence'
  })
  console.log(data)
  ```

- 生成指定长度的标题和句子

  ```js
  const data = Mock.mock({
      title: "@ctitle(8)",
      sentence: '@csentence(50)'
  })
  ```

- 生成指定范围的

  ```js
  const data = Mock.mock({
      title: "@ctitle(5,8)",
      sentence: '@csentence(50,100)'
  })
  console.log(data)
  ```

### 生成段落

- 随机生成段落

  ```js
  const data = Mock.mock({
      content: '@cparagraph()' 
  })
  console.log(data)
  ```

### 生成数字

- 生成指定数字

  ```js
  const data = Mock.mock({
      "number|80": 1
  })
  console.log(data)
  ```

- 生成范围数字

  ```js
  const data = Mock.mock({
      "number|1-999": 1
  })
  console.log(data)
  ```

### 生成增量id

- 随机生成标识

  ```js
  const data = Mock.mock({
      id: '@increment()'
  })
  console.log(data)
  ```

### 生成姓名-地址-身份证号

- 随机生成姓名-地址-身份证号

  ```js
  const data = Mock.mock({
      name: '@cname()',
      idCard: '@id()',
      address: '@city(true)' // 如果@city(),只会生成市，如果@city(true)会生成省和市
  })
  console.log(data)
  ```

### 随机生成图片

- 生成图片：

- 参数1：图片可选大小

- 参数2：图片背景色

- 参数3：图片前景色

- 参数4：图片格式

- 参数5：图片文字

  ```js
    const data = Mock.mock({
        image: "@image('300x300', '#50B347', '#FFF', 'Mock.js')"
    })
    console.log(data)
  ```

### 生成时间

- @Date
- 生成指定格式时间：@date(yyyy-MM-dd hh:mm:ss)

    ```js
    const time = Mock.mock({
        time1: '@date()', // 只有年月日
        time2: '@date(yyyy-MM-dd hh:mm:ss)'
    })
    console.log(time) 
    ```

### 指定数组返回的条数

- 指定长度：'data|5'

- 指定范围：'data|5-10'

  ```js
  const data = Mock.mock({
      'list|50-99':[
          {
              name: '@cname()',
              address: '@city(true)',
              id: '@increment(1)' // 每次都增加1
          }
      ]
  })
  ```

## mock拦截请求

- 在项目中安装axios

  ```js
  npm install axios
  ```

- 在 main.js 文件引入

    ```js
    import axios from 'axios'
    ```

- 在mock文件夹的`index.js`文件中写mock语法

### 定义不携带参数的get请求

```js
Mock.mock('/api/get/user','get',()=>{
    return {
        status: 200,
        message: '获取新闻列表数据成功'
    }
})
```


### 定义post请求

```js
Mock.mock('/api/post/user','post',()=>{
    return {
        status: 200,
        message: '添加新闻列表数据成功'
    }
})
```

- 在 App.vue 文件发送请求
```vue
<template>
  <div id="app">
    
  </div>
</template>

<script>
export default {
  created() {
        axios.get('/api/get/user')
          .then(function (response) {
          console.log(response);
       }),

        axios.post('/api/post/user')
          .then(res => console.log(res)),
    }
}
</script>

<style lang="less" scoped>

</style>
```
