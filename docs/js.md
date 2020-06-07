js

## 基础

### 输出打印信息

#### 控制台

```js
console.log()
console.dir()
console.table()
console.warm()
// 计算出time/timeEnd中间所有执行程序所消耗的时间
console.time('AA')
console.timeEnd('AA')
```

#### window

alert/confirm/prompt弹出的内容 默认 会转化为字符串(toString)

```js
alert() 
alert(1) // "1"
alert(null) // "null"
alert([1,2]) // "1, 2"
alert({name: 'yx'}) // "[object Object]"

// 有确认和取消
confirm()
let flag = confirm('是真的？')
console.log(flag) // 确认 =》 true, 取消 =》 false

// 含输入框，确认，取消
prompt()
let content = prompt('请输入姓名')
console.log(content) //确认 =》 输入内容, 取消 =》 null
```

#### 页面

与alert一样，先转化为字符串再输出

```js
document.write({name: 'yx'}) //"[object Object]"
innerHTML/innerText ：会将之前容器的内容覆盖，如果要追加，采用+=
```

### number数据类型

Infinity / NaN / .... 都属于number类型

```js
console.log(NaN == NaN) // false，NaN 永远和谁都不相等，包括自己
```

isNaN 用于验证是否是一个数字

如果`isNaN`函数的参数不是`Number`类型， `isNaN`函数会首先尝试将这个参数转换为数值，然后才会对转换后的结果是否是[`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)进行判断

```js
console.log(isNaN(Infinity)) // true
console.log(isNaN(NaN)) // false
```

![1591354090989](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591354090989.png)![1591353637740](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591353637740.png)

![1591354083668](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591354083668.png)

![1591355211181](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591355211181.png)![1591372387435](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591372387435.png)![1591408102828](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591408102828.png)![1591408420547](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591408420547.png)

![](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591347776495.png)![1591348242846](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591348242846.png)![1591426652509](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591426652509.png)![1591409756990](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591409756990.png)

tofixed(2): 保留小数位，结果是字符串

### String数据类型

把其他数据类型转换为字符串类型

String([value])

[value].toString()

`普通对象转换为字符串都是"[object Object]"，数字对象转换为字符串是"第一项，第二项"`

```js
obj.toString()
"[object Object]"

['a','f','g'].toString()
"a,f,g"

var func = function(){}
func.toString()
"function(){}"
```



![1591365406376](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591365406376.png)

![1591364702884](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591364702884.png)



### Boolean数据类型

![1591370780352](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591370780352.png)![1591368085201](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591368085201.png)

规则：只有`0/NaN/null/undefined/空字符串`最后是false，其余的都是true

### 练习题

![1591430346322](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591430346322.png)![1591430668347](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591430668347.png)![1591430939346](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591430939346.png)![1591431031795](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591431031795.png)![1591431243081](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591431243081.png)![1591431499682](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591431499682.png)

### == 和 === 区别

==：将字符串转为数字

![1591408646805](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591408646805.png)![1591502045102](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591502045102.png)![1591408769293](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591408769293.png)![1591422206788](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591422206788.png)![1591502357822](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591502357822.png)

### undefined与null

  * undefined代表没有赋值
  * null代表赋值了, 只是值为null

### 三元运算符

条件 	? 	成立处理的时间	:	不成立做得事情

在条件成立或不成立的时候，如果不想做一些事情，则使用null来占位

```js
x === 10 ? x++ : null
```

如果需要做多捡事情，则用小括号包起来，每件事情中间用逗号分隔

```js
x > 0 ? (x++， console.log（x)): null
```

### 判断语句

switch case：每一种case情况都是基于 === 进行比较的（严格比较，需保证数据类型都一致）

![1591422960970](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591422960970.png)

### break、continue和return

break：强制结束整个循环（循环体中一旦遇到break，整个循环都结束，break下面代码不再执行，步长累计也不再执行）

continue：结束本轮循环，下一轮继续（循环体中一旦遇到continue，本轮循环结束，continue下面代码不再执行，但是步长会累计执行）

`注：break和continue只能在循环中使用，用于结束循环；return只能在函数中使用，用于返回信息并告知函数体下面代码`

![1591425833106](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591425833106.png)

![1591424894443](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591424894443.png)![1591425233115](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591425233115.png)

### 总结

![1591446628770](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591446628770.png)![1591446863759](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591446863759.png)

## 数组

### 遍历数组for

就是把数组的元素从头到尾访问一次，数组索引号从0开始

### 数组转字符串

['red', 'green', 'blue', 'pink']  ==>  'red|green|blue|pink'

```js
var arr = ['red', 'green', 'blue', 'pink'];
var str = '';
var sep = '*';
for (var i = 0; i < arr.length; i++) {
    str += arr[i] + sep;
}
console.log(str);
```

### 新增数组元素(请转到webapi)

```js
// 1. 新增数组元素 修改length长度 
var arr = ['red', 'green', 'blue'];
arr.length = 5; // 把我们数组的长度修改为了 5  里面应该有5个元素 
console.log(arr[3]); // undefined
// 2. 新增数组元素 修改索引号 追加数组元素
var arr1 = ['red', 'green', 'blue'];
arr1[3] = 'pink';
arr1[0] = 'yellow'; // 这里是替换原来的数组元素
arr1 = '有点意思';
console.log(arr1); // 不要直接给 数组名赋值 否则里面的数组元素都没有了
```

### 删除指定元素(请转到webapi)

## 函数

低耦合，高内聚

![1591494567520](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591494567520.png)

### 概念

函数使用分为两步： 声明函数 和 调用函数

`调用函数的时候千万不要忘记加小括号`

```js
// 1. 声明函数
function 函数名() {
    // 函数体
}

//2. 调用函数
// 函数名();
sayHi();
```



### 函数的使用

- 函数是可以重复相同的代码

```js
function cook() {
  console.log('酸辣土豆丝');
}
cook();
cook();
```



- 我们可以利用函数的**参数**实现函数重复不同的代码

```js
function 函数名(形参1, 形参2...) {} // 在声明函数的小括号里面是 形参 （形式上的参数）

函数名(实参1, 实参2...); // 在函数调用的小括号里面是实参（实际的参数）
```



- 形参和实参的执行过程

有function先跳过，待调用函数的时候再回去函数中执行

![1591247736831](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591247736831.png)



### 函数参数

- 如果实参的个数和形参的个数一致 则正常输出结果
- 如果**实参的个数多于形参的个数**  会取到形参的个数【即：形参有多少个，就取实参从左到右的多少个】
- 如果**实参的个数小于形参的个数**  多于的形参的值定义为undefined

```js
// 函数形参实参个数匹配
function getSum(num1, num2) {
  console.log(num1 + num2);
}
// 1. 如果实参的个数和形参的个数一致 则正常输出结果
getSum(1, 2);
// 2. 如果实参的个数多于形参的个数  会取到形参的个数 
getSum(1, 2, 3);
// 3. 如果实参的个数小于形参的个数  多于的形参定义为undefined  最终的结果就是 NaN
getSum(1); // NaN
```

![1591496289349](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591496289349.png)

### 函数的返回值return

```js
函数的返回值格式
function 函数名() {
  return 需要返回的结果;
}
```

(1) 函数只是实现某种功能，**最终的结果需要返回给函数的调用者**	调用者：**函数名()** 通过return 实现的

(2) 只要函数遇到return 就把后面的结果 返回给函数的调用者  函数名() = return后面的结果

(3) 通常用一个变量来接收函数返回的值

```js
function getResult() {
  return 666;
}
const result = getResult(); // getResult()   = 666, 不会输出结果到控制台
console.log(result);
console.log(getResult());
```

![1591497815590](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591497815590.png)![1591498210863](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591498210863.png)

#### return

- return 终止函数：return 后面的代码不会被执行
- return 只能返回一个值：返回的结果是**最后一个值**
- 如果需要返回多个值，可以返回数组return [num1 + num2, num1 - num2]
- 函数如果有return 则返回的是 return 后面的值，如果函数没有 return 则返回undefined

```js
// 1. return 终止函数
function getSum(num1, num2) {
    return num1 + num2; // return 后面的代码不会被执行
    alert('我是不会被执行的哦！')
}
console.log(getSum(1, 2));

// 2. return 只能返回一个值
function fn(num1, num2) {
    return num1, num2; // 返回的结果是最后一个值
}
console.log(fn(1, 2));

// 3.  我们求任意两个数的 加减乘数结果
function getResult(num1, num2) {
    return [num1 + num2, num1 - num2, num1 * num2, num1 / num2];
}
var re = getResult(1, 2); // 返回的是一个数组
console.log(re);

// 4. 我们的函数如果有return 则返回的是 return 后面的值，如果函数没有 return 则返回undefined
function fun1() {
    return 666;
}
console.log(fun1()); // 返回 666
function fun2() {

}
console.log(fun2()); // 函数返回的结果是 undefined
```



#### break/continue/return区别

break：结束当前的循环体（for，while）

continue：跳出本次循环，继续执行下次循环（for，while）

return：不仅可以退出循环，还能返回return语句中的值，同时还可以结束当前的函数体内的代码



### 函数特有arguments

- 只有函数才有 arguments对象  而且是每个函数都内置好了这个arguments

- arguments面存储了**所有传递过来的实参**

```js
function fn() {
  console.log(arguments);
  console.log(arguments.length);
  for (var i = 0; i < arguments.length; i++) {
      console.log(arguments[i]);
  }
}
fn(1, 2, 3, 4, 5);
```

![1591497109212](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591497109212.png)

- arguments**展现形式是伪数组**,并不是真正意义上的数组

1. 具有数组的 length 属性

2. 按照索引的方式进行存储的

3. 它没有真正数组的一些方法 **pop**()  **push**() 等等

![](https://i.bmp.ovh/imgs/2020/06/9a70ee19e2c57b9c.png)

```js
function getRandomMax(){ // arguments => [1, 2, 3]
  let max = arguments[0]
  for(let k = 0; k < arguments.length ; k++ ){
    if(arguments[k] > max){
      max = arguments[k]
    }
  }
  return max
}

const res1 = getRandomMax(1, -1, 6)
const res2 = getRandomMax(12, 9, -1, 6)
console.log(res1, res2)
```

![1591497448010](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591497448010.png)

### 函数相互调用

#### demo

- demo1

fn1中调用fn2

```js
function fn1() {
  console.log(111);
  fn2();
  console.log('fn1');
}

function fn2() {
  console.log(222);
  console.log('fn2');
}
fn1();
```

![](https://i.bmp.ovh/imgs/2020/06/fc9c5dd8a9e6aba2.png)



- 用户输入年份，输出当前年份2月份的天数

```js
function isLeap(year){
  if((year%4===0 && year%100!==0) || year % 400 ===0){
    return year + '是闰年'
  }
  return year + '不是闰年'
}

function getMonth(year){
  // let year = prompt('请输入年份')
  if(isLeap(year)){
    return year + '年这个月有29天'
  }
  return year + '年这个月有28天'
}
console.log(getMonth(2016));
```



### 函数两种声明方式

#### 命名函数

1. 利用函数关键字自定义函数(命名函数)

```js
function fn() {}
fn();
```

#### 匿名函数

1. 函数表达式(匿名函数) 

(1) fun是**变量名** 不是函数名  

(2) 函数表达式声明方式跟声明变量差不多，只不过变量里面存的是值 而 函数表达式里面存的是函数

(3) 函数表达式也可以进行传递参数

```js
var 变量名 = function() {};
var fun = function(aru) {
    console.log('我是函数表达式');
    console.log(aru);
}
fun('pink老师');
```



### 作用域和变量

#### 全局作用域

全局作用域： 整个script标签 或者是一个单独的js文件

```
var num = 10;
console.log(num);
```

#### 局部作用域

局部作用域（函数作用域） 在函数内部就是局部作用域 这个代码的名字只在函数内部起效果和作用

```js
function fn() {
  // 局部作用域
  var num = 20;
  console.log(num);
}
fn();
```



#### 全局变量

变量的作用域： 根据作用域的不同我们变量分为全局变量和局部变量

在全局作用域下的变量 在全局下都可以使用， 全局变量只有浏览器关闭的时候才会销毁，比较占内存资源

注意：如果在函数内部，**没有声明直接赋值的变量也属于全局变量**

```js
var num = 10; // num就是一个全局变量
console.log(num);

function fn() {
    console.log(num);
}
fn();
```



#### 局部变量

局部变量：在函数内部的变量， 当程序执行完毕就会销毁， 比较节约内存资源

`注意： 函数的形参也可以看做是局部变量`

```js
function fun(aru) {
    var num1 = 10; // num1就是局部变量 只能在函数内部使用
    num2 = 20;
}
fun();
console.log(num1); // 报错
console.log(num2); // 20
```



#### 块级作用域{}

有{}的就代表是块级作用域，例：if{}, for{}

在js中没有块级作用域，在es6才有块级作用域



### 作用域链

内部函数访问外部函数的变量，采取的是链式查找的方式来决定取那个值，这种结构我们称为作用域链

作用域链采取**就近原则**

案例

```js
function f1() {// 外部函数
  var num = 123;

  function f2() {// 内部函数
      var num = 0;
      console.log(num); // 站在目标出发，一层一层的往外查找
  }
  f2();
}
var num = 456;
f1();
```

![](https://i.bmp.ovh/imgs/2020/06/7bbe71dda2d7f41d.png)



### 预解析

js引擎运行js 分为两步：  预解析  代码执行

(1). 预解析 js引擎会把js 里面所有的 var  还有 function 提升到当前作用域的最前面

(2). 代码执行  按照代码书写的顺序从上往下执行

四问

```js
// 1问  
console.log(num); // 报错未定义

// 2问
console.log(num); // undefined  坑
var num = 10;
===========================
    解析：变量会提升
var num；
console.log(num);
num = 10;
***************************
    
// 3问  
fn(); // 输出结果
function fn() {
   console.log(11);
}
===========================
    解析：函数会提升
function fn() {
   console.log(11);
}
fn();
***************************

// 4问
fun(); // 报错  坑
var fun = function() {
   console.log(22);
}
===========================
    解析：变量会提升
var fun；
fun(); // 报错  坑
fun = function() {
   console.log(22);
}
```



#### 变量预解析（变量提升）

变量提升 就是`把所有的变量声明提升到当前的作用域最前面,不提升赋值操作`

#### 函数预解析（函数提升）

函数提升 就是`把所有的函数声明提升到当前作用域的最前面,不调用函数`

#### 案例

案例1

```js
var num = 10;
fun();

function fun() {
  console.log(num);
  var num = 20;
}
========================
    解析1：
var num;
function fun() {
  console.log(num);
  var num = 20;
}
num = 10;
fun();

	解析2：
var num;
function fun() {
  var num；
  console.log(num);
  num = 20;
}
num = 10;
fun();
```

案例2

```js
var num = 10;
function fn() {
    console.log(num);
    var num = 20;
    console.log(num);
}
fn();
=========================
    解析
var num;
function fn() {
    var num;
    console.log(num);
    num = 20;
    console.log(num);
}
num = 10;
fn();
```

案例3

```js
f1();
console.log(c);
console.log(b);
console.log(a);

function f1() {
  var a = b = c = 9;
  console.log(a);
  console.log(b);
  console.log(c);
}
================================
    解析：
function f1() {
  var a;
  a = b = c = 9;
  // 相当于 var  a  = 9; b = 9; c = 9; b 和 c 直接赋值 没有var 声明 当 全局变量看
  // 集体声明  var a = 9, b = 9, c = 9;
  console.log(a);
  console.log(b);
  console.log(c);
}
f1();
console.log(c);
console.log(b);
console.log(a);
```

## 对象

### 创建对象

#### 对象字面量创建对象(不能复用)

```js
var obj = {};  // 创建了一个空的对象 
```

(1) 里面的属性或者方法我们采取键值对的形式  键 属性名 ： 值  属性值 

(2) 多个属性或者方法中间用**逗号隔开**的

(3) 方法冒号后面跟的是一个匿名函数

(4)`key不能是引用数据类型的`，value可以是任何的数据类型

```js
var obj = {
  uname: '张三疯',
  age: 18,
  sayHi: function() {console.log('hi~');}
}
```

使用对象

调用对象的属性：对象名.属性名	/	对象名['属性名']

```js
console.log(obj.uname);
console.log(obj['age']);
```

调用对象的方法：对象名.方法名()

```js
obj.sayHi();
```

#### new Object 创建对象(不能复用)

- 利用 等号 = 赋值的方法 添加对象的属性和方法
- 每个属性和方法之间用**分号结束**

```js
var obj = new Object(); // 创建了一个空的对象
obj.uname = '张三疯';
obj.age = 18;
obj.sayHi = function() {
  console.log('hi~');
}
console.log(obj.uname);
obj.sayHi();
```

#### 构造函数创建对象

- 构造函数名字**首字母要大写**
- 构造函数**不需要return 就可以返回结果**
- 调用构造函数**必须使用 new**
- 只要new Star() 调用函数就创建一个对象
- 属性和方法前面**必须添加 this**

```js
function 构造函数名() {
    this.属性 = 值;
    this.方法 = function() {}
}
new 构造函数名();
```

案例

```js
function Star(uname, age) {
  //var this = new Object() 默认内部处理
  this.name = uname;
  this.age = age;
  this.sing = function(sang) {
    console.log(sang);
  }
    // return this； 默认返回this
}
var ldh = new Star('刘德华', 18);
ldh.sing('冰雨');
```

### 键值对的增删改和获取

```js
var obj = {}
// 添加
obj['a'] = 'aa'
// 修改
obj['a'] = 'bb'
//删除:假删除置为null，真删除delete
obj['a'] = null
delete obj['a']

//获取
//获取指定属性名的属性值
console.log(obj['a'])
//指定的属性不存在，获取到的属性值是undefined，不会报错
console.log(obj['age'])
//获取当前对象中  所有的属性名，返回结果数所有属性名的数组
console.log(Object.keys(obj)) // =>['a']
```

### key不能是引用类型

对象[属性名] = 属性值（属性名是具体的值，如果是对象则先转换为字符串）

对象中的属性名不能重复，而且数字和字符串如果相同算同一个属性，例：obj[0]=obj['0']

![1591372109649](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591372109649.png)

### new关键字

执行过程

1. 先内存中创建了一个新的空对象

2. new会让this指向这个新对象

3. 执行构造函数里面的代码，给这个空对象添加属性和方法

4. 返回这个对象(返回this)

![](https://i.bmp.ovh/imgs/2020/06/27d2bba8ba8eb2a9.png)

### this关键字

### 遍历对象for...in

遍历循环对象时，会优先按照从小到大的机制遍历数字属性的

![1591425677005](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591425677005.png)

```js
var obj = {
  name: 'pink老师',
  fn: function() {}
}

for (var k in obj) {
  console.log(k); // k 变量 输出  得到的是 属性名
  console.log(obj[k]); // obj[k] 得到是 属性值
}
```

### 总结

![1591448003170](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591448003170.png)

## 内置对象

### Math

#### 绝对值abs

传入一个非数字形式的字符串或者 undefined/empty 变量，将返回 `NaN`。传入 null 将返回 0。

```js
Math.abs('-1');     // 1
Math.abs(null);     // 0
Math.abs("string"); // NaN
```

#### 向下取整floor

#### 向上取整ceil

#### 四舍五入round

```js
console.log(Math.round(-1.5)); // 这个结果是 -1
```

#### 最大值max

#### 最小值min

#### 幂次方pow

```js
console.log(Math.pow(7, 3));
// expected output: 343
```

#### 平方根sqrt

```js
Math.sqrt(9); // 3
Math.sqrt(2); // 1.414213562373095
```

#### 随机数random

函数返回一个浮点,  伪随机数在范围从**0到**小于**1**，也就是说，从0（包括0）往上，但是不包括1（排除1）

#### 随机整数

得到两个数之间的值，包括两个数在内

```js
function getRandom(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min; //含最大值，含最小值 
}
```

##### 随机取名

```js
function getRandom(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min; //含最大值，含最小值 
}
var arr = ['张三', '张三丰', '李四', '李思思'];
console.log(arr[getRandom(0, arr.length - 1)]);
```

##### 猜数字

### Date

是构造函数，必须new

#### 获取毫秒数(时间戳)

```js
// 1. 通过 valueOf()  getTime()
var date = new Date();
console.log(date.valueOf()); // 就是 我们现在时间 距离1970.1.1 总的毫秒数
console.log(date.getTime());

// 2. 简单的写法 (最常用的写法)
var date1 = +new Date(); // +new Date()  返回的就是总的毫秒数
console.log(date1);

// 3. H5 新增的 获得总的毫秒数
console.log(Date.now());
```

#### 倒计时案例

1.核心算法：输入的时间减去现在的时间就是剩余的时间，即倒计时 ，但不能拿着时分秒相减，比如 05 分减去25分，结果会是负数的。

2.用时间戳来做。用户输入时间总的毫秒数减去现在时间的总的毫秒数，得到的就是剩余时间的毫秒数。

3.把剩余时间总的毫秒数转换为天、时、分、秒 （时间戳转换为时分秒）

转换公式如下： 

 d = **parseInt**(总秒数/ 60/60 /24);    *//  计算天数*

 h = **parseInt**(总秒数/ 60/60 %24)   *//   计算小时*

 m = **parseInt**(总秒数 /60 %60 );     *//   计算分数*

 s = **parseInt**(总秒数%60);            *//   计算当前秒数*

```js
function countDown(time) {
  var nowTime = +new Date(); // 返回的是当前时间总的毫秒数
  var inputTime = +new Date(time); // 返回的是用户输入时间总的毫秒数
  var times = (inputTime - nowTime) / 1000; // times是剩余时间总的秒数 
  var d = parseInt(times / 60 / 60 / 24); // 天
  d = d < 10 ? '0' + d : d;
  var h = parseInt(times / 60 / 60 % 24); //时
  h = h < 10 ? '0' + h : h;
  var m = parseInt(times / 60 % 60); // 分
  m = m < 10 ? '0' + m : m;
  var s = parseInt(times % 60); // 当前的秒
  s = s < 10 ? '0' + s : s;
  return d + '天' + h + '时' + m + '分' + s + '秒';
}
console.log(countDown('2019-5-1 18:00:00'));
var date = new Date();
console.log(date);
```

### Array(16个)

![1591523080931](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591523080931.png)

![1591504262837](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591504262837.png)

#### 检测是否为数组

(1) instanceof  运算符 它可以用来检测是否为数组

```js
var arr = [];
console.log(arr instanceof Array);
```

(2) Array.isArray(参数);  H5新增的方法  ie9以上版本支持

```js
var arr = [];
console.log(Array.isArray(arr));
```

#### 增删改

push()：push完毕之后，返回的结果是**新数组的长度 **，原数组也会发生变化

![1591508683924](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591508683924.png)

unshift()：unshift完毕之后，返回的结果是**新数组的长度** ，原数组也会发生变化

![1591508816058](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591508816058.png)

pop()：pop完毕之后，返回的结果是**删除的那个元素**  ，原数组也会发生变化

![1591508759693](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591508759693.png)

shift()：shift完毕之后，返回的结果是**删除的那个元素**  ，原数组也会发生变化

![1591508849835](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591508849835.png)

splice()：实现增删改，会影响原数组

![1591509397383](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591509397383.png)![1591509404739](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591509404739.png)

#### 排序

reverse()：翻转数组

![1591513149023](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591513149023.png)

sort()：数组排序(冒泡排序)

return 	A-B：升序排列		B - A：降序排列		A.length - B.length：按长度排列

![1591513962155](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591513962155.png)

![1591513608164](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591513608164.png)

#### 验证是否包含某一项

返回数组元素索引号方法,只返回第一个满足条件的索引号,如果在该数组里面找不到元素，则返回的是 -1

indexOf()

lastIndexOf()

includes()

#### 数组转字符串

toString()

```js
var arr = [1, 2, 3];
console.log(arr.toString()); // 1,2,3
```

![1591510848907](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591510848907.png)

join()

```js
var arr1 = ['green', 'blue', 'pink'];
console.log(arr1.join()); // green,blue,pink
```

![1591511241092](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591511241092.png)

#### 拼接

concat()：返回新数组

![1591510700883](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591510700883.png)

#### 截取

slice()：返回被截取的新数组

![1591510415079](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591510415079.png)

#### 数组迭代

forEach

![1591515072719](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591515072719.png)

map

![1591514581252](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591514581252.png)

#### 数组去重

##### ES6 Set去重

```js
function unique (arr) {
  return Array.from(new Set(arr))
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
console.log(unique(arr))
 //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {}, {}]
```

##### 双重for循环

```js
function unique(arr){            
        for(var i=0; i<arr.length; i++){
            for(var j=i+1; j<arr.length; j++){
                if(arr[i]==arr[j]){ //第一个等同于第二个，splice方法删除第二个
                    arr.splice(j,1);
                    j--;
                }
            }
        }
return arr;
}
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{},{}];
    console.log(unique(arr))
    //[1, "true", 15, false, undefined, NaN, NaN, "NaN", "a", {…}, {…}]     //NaN和{}没有去重，两个null直接消失了
```

![1591523068915](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591523068915.png)

### String

#### 字符返回位置

indexOf()

lastIndexOf()

##### demo

!> 思考：`查找字符串"abcoefoxyozzopp"中所有o出现的位置以及次数`

思路：先查找第一个o出现的位置，只要indexOf 返回的结果不是 -1 就继续往后查找，因为indexOf 只能查找到第一个，所以后面的查找，一定是当前索引加1，从而继续查找

```js
var str = "oabcoefoxyozzopp";
var index = str.indexOf('o');
var num = 0;
while (index !== -1) {
    console.log(index);
    num++;
    index = str.indexOf('o', index + 1);
}
console.log('o出现的次数是: ' + num);
```

#### 位置返回字符

charAt(index)：根据位置返回字符

str[index]：根据位置返回字符

```js
var str = 'andy';
console.log(str.charAt(3)); // y
```

!> 思考：`打印出字符串中所有的字符`

```js
var str = 'andy';
// 遍历所有的字符
for (var i = 0; i < str.length; i++) {
    console.log(str.charAt(i));
}
```

charCodeAt(index)：返回相应索引号的字符ASCII值【用于判断用户按下哪个键】

```js
var str = 'andy';
console.log(str.charCodeAt(0)); // 97
```

!> 思考：`统计字符串出现最多的字符和次数`

#### 字符串操作方法

##### 拼接

concat('字符串1','字符串2'....)

##### 截取

substr('截取的起始位置', '截取几个字符')

substr和substring都是截取字符串中子串

（1）substr(start,length) 返回从start位置开始length长度的子串

（2）substring(start,end) 返回从start位置开始到end位置的子串（不包含end）



##### 替换

replace('被替换的字符', '替换为的字符')，只会替换第一个字符

##### 字符转换为数组

split('分隔符')

##### 转大写

toUpperCase()

##### 转小写

toLowerCase()

### 数据类型

#### 检测数据类型

##### typeof

返回结果是一个字符串

特殊检测结果

```js
typeof NaN/Infinity 			=> "number"
typeof null/Array 				=> "object"
typeof 普通对象/数组对象/正则对象 	=>"object"
```



```js
console.log(typeof []) // "object"
console.log(typeof typeof typeof []) // 从后往前看
解析：
typeof typeof typeof [] =>"object"
typeof typeof "string" => "string"
typeof "string" => "string"
```



#### 堆和栈

![](https://i.bmp.ovh/imgs/2020/06/c8d7f3cb06b23bd5.png)

#### 简单数据类型

值类型，在存储变量中存储的是值本身

例：String, number, boolean, undefined, null

**简单数据类型是存放在栈里面，里面直接开辟一个空间存放的是值**

![](https://img-blog.csdnimg.cn/20190401214732709.png)

#### 复杂数据类型

引用类型，在存储变量中存储的是地址(引用)

例：通过new关键字创建的对象Object，如Object, Array, Date

**复杂数据类型首先在栈里面存放地址 `十六进制表示`  然后这个地址指向堆里面的数据**

![](https://img-blog.csdnimg.cn/20190401214757884.png)

案例

![1591343101735](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591343101735.png)![1591343927913](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591343927913.png)

案例1![1591346034637](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591346034637.png)

案例2

![1591346479623](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591346479623.png)

案例3

![1591346790413](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591346790413.png)

### 练习题

案例1：

b++: b = 1 相当于创建了一个新的值

![1591409330950](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591409330950.png)

案例2

![1591410161902](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591410161902.png)

案例3

console.log(a.x) = "undefined", 因为这个是获取值而不是新建或者修改值

![1591410855496](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591410855496.png)

### 总结

![1591447963733](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591447963733.png)

## WebApi

### DOM

![1591436841366](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591436841366.png)

![1591436661212](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591436661212.png)



#### 循环点击事件bug

![1591456670325](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1591456670325.png)



### BOM

#### 鼠标事件

click：单击事件
dblclick：双击事件
mousedown：按下鼠标键时触发
mouseup：释放按下的鼠标键时触发
mousemove：鼠标移动事件
mouseover：移入事件
mouseout：移出事件
mouseenter：移入事
mouseleave：移出事件
contextmenu：右键事件