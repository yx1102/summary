# css

## 垂直居中

![](https://i.bmp.ovh/imgs/2020/05/4a6283f40f452685.png)

详情：https://www.bilibili.com/video/BV1XJ411X7Ud?p=73

```html
<style>
  .box1 {width: 500px;height: 500px;background-color: #bfa;position: relative;}
  .box2 {width: 100px;height: 100px;background-color: orange;position: absolute;
    margin: auto;left: 0;right: 0;top: 0;bottom: 0;}
</style>

<div class="box1">
    <div class="box2"></div>
</div>
```



## 轮播图

![](https://i.bmp.ovh/imgs/2020/05/03ad47f76e226c35.png)

图片：ul>li>img		图片叠在一起需设置absolute

指示点：div>a			指示点在图片上面需设置z-index和absolute；指示点有样式会超出范围background-clip

```html

```



## 图片下间隙

```html
<style>
img{
   vertical-align: bottom/top/middle; /*有 vertical-align 就可以，无论设置的是什么值*/
}
</style>

<p>
   <img src="./img/an.jpg" alt=""> 
</p>
```



## 显示省略号

```css
p{
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    line-clamp: 2;
}
```

