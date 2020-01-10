## js简史

#### js的组成部分

+ 核心：描述该语言该的语法和基本对象
+ 文档对象模型（Dom）：描述与网页内容进行交互的对象和接口
+ 浏览器对象模型（Bom）：描述与浏览器交互的对象和接口

####  事件循环

> ##### **科普**
>
> 1. v8引擎里面没有ajax、console、settimeout……，他里面只有两个东西
>
>    + 堆
>
> + 调用栈
>
> 2. chrome 浏览器里面包含V8引擎、webAPI、事件队列
>
> 3. 单线程就是只有一个调用栈，同一时间只能执行一个事件
> 4. 程序执行顺序:
>
> + 入口进栈
> + 申明所有东西
> + 没有申明的变量，然后开始从上到下执行，遇到调用函数就进栈
> + 重复2和3的步骤
>
> 3. 同步:前面的东西没有执行完，就不会执行下面的操作
> 4. 异步:前面的东西正在执行，但是我还是要先继续往后面执行操作




#### 数组

**数组去重：**

1. Array.from(new Set([...arr1,...arr2]))

**判断数组中是否包含另一个数组中的元素**

1. find

```js
var arr=[1,2,3,2,1];
var target=[3,2,1,4,6];
target.find(value=>{
    // if(arr.indexOf(value)>-1){
    //     console.log(value)//3,2,1
    // }
   return arr.indexOf(value)>-1//返回找到的第一个元素值
})
```

2. findIndex

```js
var arr=[1,2,3,2,1];
var target=[3,2,1,4,6];
target.findIndex(value=>{
    return arr.indexOf(value)>-1
})//返回找到的第一个元素的数组下标
```

**找到两个数组共同的部分**

```js
let [res,arr,target]=[[],[1,2,3],[2,3.4]];
target.find(value=>{
    if(arr.indexOf(value)>-1&&res.indexOf(value)<0){
        res.push(value)
    }
})
```

```js
getIntersection = (arr1, arr2) => {
  let len = Math.min(arr1.length, arr2.length)
  let i = -1
  let res = []
  while (++i < len) {
    const item = arr2[i]
    if (arr1.indexOf(item) > -1) res.push(item)
  }
  return res
}
```

#### 时间

```js
//1.都是用于计算时间戳
new Date('04 Dec 1995 00:12:00 GMT').getTime()===Date.parse('04 Dec 1995 00:12:00 GMT')===Date.parse('1995-12-04')
//2.通过时间戳计算日期
new Date(818035200000)//Mon Dec 04 1995 08:00:00 GMT+0800 (中国标准时间)
//3.判断历史日期
Date.parse(new Date())>Date.parse('2995-12-04')//false：这是未来的日期；true表示这是历史日期（包含今日）
//4.计算相对时间的函数
getRelativeTime = timeStamp => {
  // 判断当前传入的时间戳是秒格式还是毫秒
  const IS_MILLISECOND = isMillisecond(timeStamp)
  // 如果是毫秒格式则转为秒格式
  if (IS_MILLISECOND) Math.floor(timeStamp /= 1000)
  // 传入的时间戳可以是数值或字符串类型，这里统一转为数值类型
  timeStamp = Number(timeStamp)
  // 获取当前时间时间戳
  const currentTime = Math.floor(Date.parse(new Date()) / 1000)
  // 判断传入时间戳是否早于当前时间戳
  const IS_EARLY = isEarly(timeStamp, currentTime)
  // 获取两个时间戳差值
  let diff = currentTime - timeStamp
  // 如果IS_EARLY为false则差值取反
  if (!IS_EARLY) diff = -diff
  let resStr = ''
  const dirStr = IS_EARLY ? '前' : '后'
  // 少于等于59秒
  if (diff <= 59) resStr = diff + '秒' + dirStr
  // 多于59秒，少于等于59分钟59秒
  else if (diff > 59 && diff <= 3599) resStr = Math.floor(diff / 60) + '分钟' + dirStr
  // 多于59分钟59秒，少于等于23小时59分钟59秒
  else if (diff > 3599 && diff <= 86399) resStr = Math.floor(diff / 3600) + '小时' + dirStr
  // 多于23小时59分钟59秒，少于等于29天59分钟59秒
  else if (diff > 86399 && diff <= 2623859) resStr = Math.floor(diff / 86400) + '天' + dirStr
  // 多于29天59分钟59秒，少于364天23小时59分钟59秒，且传入的时间戳早于当前
  else if (diff > 2623859 && diff <= 31567859 && IS_EARLY) resStr = getDate(timeStamp)
  else resStr = getDate(timeStamp, 'year')
  return resStr
}

```

#### 函数处理

```js
/**
 * @description 绑定事件 on(element, event, handler)
 */
export const on = (function () {
  if (document.addEventListener) {
    return function (element, event, handler) {
      if (element && event && handler) {
        element.addEventListener(event, handler, false)
      }
    }
  } else {
    return function (element, event, handler) {
      if (element && event && handler) {
        element.attachEvent('on' + event, handler)
      }
    }
  }
})()
```

`navigator.language获取浏览器系统语言`

+ 文档对象模型（DOm）：描述与网页内容进行交互的对象和接口
+ 浏览器对象模型（Bom）：描述与浏览器交互的对象和接口

#闭包练习题

1. 没有形参

```js
/* 首先有变量提升var n;var c;还有函数申明a,然后代码从上到下依次执行 */
var n=0;/* 1 */
function a(){/* 3 形成私有作用域*/
/* 没有形参，有变量提升var n;函数申明，然后代码从上到下依次执行 */
    var n=10;/* 31   这个n是a的私有变量*/
    function b(){/* 33   形成私有作用域*/
/* 没有形参，没有有变量提升，然后代码从上到下依次执行 */
        n++;/* 331   b中没有n的变量提升，所以上级查找，a中有n的申明，所以此时的n是a里面的，n++;所以var n=10就应该是var n=11*/
        console.log(n);/* 332   输出的是a的私有变量n,因为上一步骤n=11;所以输出11*/
    }
    b();/* 32 */
    return b;/* 34 把b的函数体进行返回  */
}
var c=a();/* 2  接收的是b的函数体*/
c();/* 4    相当于执行b的函数体，继续b执行的刚刚的操作 */
console.log(n)
//结果：11,12,0
```

2. 形参

```js
// 全局作用域有变量提升var a;var b;var c;函数申明test->AAAFFF111
var a=10,b=11,c=12;/* 1 */
function test(a){/* 3 */
//私有作用域：有变量提升var b;形参a=10,所以a,b是私有的
    a=1;/* a：私有作用域可以找到，所以把a=10改为a=1 */
    var b=2;/* b：赋值 */
    c=3;/* c：私有作用域不能找到，所以到外层作用域找，所以把c=12改为了c=3 */
}
test(10);/* 2 */
console.log(a)/* 10 */
console.log(b)/* 11 */
console.log(c)/* 3 */
```

3. 条件判断

```js
// 不管条件是否成立都要先进行变量提升
// 全局作用域变量提升：var a;
if(!("a" in window)){/* "a" in window 在全局作用域下是否有a,因为在全局作用域下有var a <=>window.a='undefine',所以  "a" in window成立 ，但是取反就代表不成立，所以下面的代码不执行*/
    var a=1;
}
console.log(a)/* undefined */
```

4. arguments、没有返回值

```js
// 全局作用域变量提升：var a;函数申明b->AAAFFF111;
var a=4;
function b(x,y,a){
//私有作用域：形参x=1,y=2,a=3
console.log(a);/* 3 */
arguments[2]=10;/* 让第三个传递进来的实参等于10，看下方笔记 */
console.log(a);/* 10 */
}
a=b(1,2,3);/*  b执行返回给a ,但是b函数执行没有返回值，所以undefined*/
console.log(a);/* undefined */

/*在js的非严格模式下:
    函数的实参集合与形参的变量存在映射关系
    function fn(a){
        //=>a=10;
        arguments[0]=100;
        //=>a=100
    }
    fn(10)
  在js的严格模式下：arguments和形参变量的映射关系被切断，相互没有干扰
    "use strict"
    function fn(a){
        //=>a=10;
        arguments[0]=100;
        //=>a=10
    }
    fn(10)

 */
```

5. 运算符、立即执行函数

```js
// 全局作用域：var foo；立即执行函数不能当成函数申明
var foo='hello';/* 1 */
(function (foo){
    // 私有作用域 ：形参foo='hello'，因为已经有了形参foo了，所以var foo就不用重复声明了
    console.log(foo);/* hello */
    var foo = foo || 'world';/* 看下方笔记 ，所以foo='hello'*/
    console.log(foo)/* hello */
})(foo)/* 2 把全局foo的值当做实参传递给私有作用域的形参 */

/*
    &&逻辑与 ||逻辑或
    1.在条件判断中
        &&：所有条件为真才为真
        ||：一个为真就是真
    2.赋值操作中
        ||：a||b  首先看a的真假，(是真就返回)a为真，返回a 否则返回b,不管b的值是什么
        &&：a&&b  首先看a的真假，(是假就返回)a为假, 返回a 否则返回b，不管b的值是什么
    3.优先级：&&>||

    所以有些高级用法，判断是否为undefined，是就设置为0：
    var u;
    var a=u||0; // u=undefined，为假，所以给a=0; 类似于if(typeof u==="undefined") a=0;
    cb&&cb();// cb是一个函数，就执行；类似于if(typeof cb ==='function') cb()
 */
```

6. 闭包作用域

```js
// 全局作用域 var a;var f;fn->AAAFFF111
var a=9;/* 1 a=0 a=1*/
function fn(){/* 3 */
// 私有作用域：无
    a=0;
    return funtion(b){/*BBBFFF111 */
        // 私有作用域，形参b=5
        return b+a++;/* 先b+a,然后a++ ,因为a是全局的，此时var a=0;所以先返回b+a,然后a++=1*/
    }
}
var f=fn();/* 2 相当于BBBFFF111*/
console.log(f(5));/* 5 */
console.log(fn()(5));/*5 先AAAFFF111再 BBBFFF111*/
console.log(f(5));/*6 此时var a=1;然后执行BBBFFF111 */
console.log(a);/* 2 */
```

#数据类型练习

```js
// 私有变量和全局变量都是引用值得时候，而且指向的是相同的内存，所以两者是相互关联的
// 全局变量var ary;var res;fn->AAAFFF111
// 只要是引用类型的都要开空间
var ary=[1,2,3,4];/* ary->BBBFFF111 */
function fn(ary){
    // 私有作用域：形参ary=[1,2,3,4]->BBBFFF111,所以这个私有变量和全局变量有间接关系
    ary[0]=0;/* 所以ary=[0,2,3,4]，全局和私有的都会发生改变 */
    ary=[0];/* [0]又是一个新的数组，将会开辟一个新的内存空间，然后赋值给ary,所以ary的指向有所改变，就不再是全局的ary地址了 */
    ary[0]=100;
    return ary;/* [100] */
}
var res=fn(ary);/* [100] */
console.log(ary);/* [1,2,3,4] */
console.log(res); /*  [100]  */
```

```js
// 全局作用域，var f;fn->AAAFFF111
function fn(i){
    // 私有作用域：i=10
    return function(n){/* BBBFFF111 */
    // 私有作用域：n=20
        console.log(n + (i++))/* i,本层没有找到，就只有在上一层里面去找，然后先加加后再求n的和 31*/
    }
}
var f=fn(10);/* 执行fn,传递实参10,所以f->BBBFFF111 */
f(20);/* 执行BBBFFF111   31*/
fn(20)(40);/* 执行AAAFFF111  */
fn(30)(50);
f(30)
```

```js
// 全局变量：var num；var obj；var fn；
var num=10;
var obj={num:20}/* obj指向AAAFFF111，里面有num，fn */
obj.fn=(function(num){/* 这个立即执行函数fn指向BBBFFF111 */
// 私有作用域：num=20;
    this.num=num*3;/* this.num中的this表示window，因为立即执行函数没有调用者，所以是window */
    num++;
    return function (n){/* 新的地址CCCFFF111 */
    /* fn(5):私有作用域n=5 ;this.num中的this表示window，因为fn前面没有调用者*/
    /* obj.fn(10):私有作用域n=10 ;this.num中的this表示obj*/
        this.num+=n;
        num++;/* 让 BBBFFF111的num加加*/
        console.log(num);/*  BBBFFF111的num*/
    }
})(obj.num);
var fn=obj.fn;/* fn指向CCCFFF111 */
fn(5);
obj.fn(10)
console.log(num,obj.num)/* 65,30 */
```

```js
function Fn(){
    this.x=100;
    this.y=200;
    this.getX=function(){
        console.log(this.x)
    }
}
Fn.prototype={
    y:400,
    getX:function(){
        console.log(this.x)
    },
    getY:function(){
        console.log(this.y)
    },
    sum:function(){
        console.log(this.y+this.x)
    },
}
var f1=new Fn();
var f2=new Fn();
console.log(f1.getX===f2.getX)
console.log(f1.getY===f2.getY)
console.log(f1.__proto__.getY===Fn.prototype.getY)
console.log(f1.__proto__.getX===f2.getX)
console.log(f1.getX===Fn.prototype.getX)
console.log(f1.constructor)
console.log(Fn.prototype.__proto__.constructor)
f1.getX();
f1.__proto__.getX();
f2.getY();
Fn.prototype.getY();

```



#### 构造器和原型



每个js规定都有一个prototype属性，指向另一个对象。注意，这个prototype就是一个对象，这个对象的属性和方法都会被构造函数所拥有。

`我们可以把这些不变的方法，直接定义在prototype对象上。这样所有对象的实例就可以共享这些方法`

原型是一个对象

原型的作用就是共享方法

一般公共属性可以定义在构造函数里面，但是方法不能放在构造函数里面，，这样会使用多个不必要的内存空间，建议方法放在原型上面 

对象自身上系统自己添加一个__ptoto_ _ 指向我们构造函数的原型对象上面

查找顺序：首先在自身上面去查找，自身没有，但是如果有—proto—，那么就去prototype上面去查找

constructor就是指向的构造函数本身

```js
//在原型上面添加新的方法：
Star.prototype.sing=function(){}

//修改原来的原型对象：赋值操作，导致原型上面没有了constructor的对象，所以我们需要手动指定constructor的对象，不然找不到。
Star.prototype={
  constructor:Star，
  sing(){}
}
```

![image-20200106163817798](D:\project\ClassNotes\images\js\1578299501.png)

![image-20200106163817798](D:\project\ClassNotes\images\js\1578300440.png)

#### es5补充的东西

对象方法：

+ Object.defineProperty(obj,pro,desc)定义对象中新属性或者修改原有的属性

  ```js
  //以前设置或者添加属性的方法
  let obj ={
      name:'cq'
  }
  obj.age=23;
  obj.name:'ng'
  //现在设置属性的方法
  Object.defineProperty(obj,'age',{
      value:23
  })
  Object.defineProperty(obj,'name',{
      value:'ng'
  })
  Object.defineProperty(obj,'id',{
      writable:false//这个属性不允许重写
      enumerable：false//如果设置为false，则不允许这个对象的该属性遍历出来，意思是keys获取不到该属性
      configurable:false//false，表示不允许删除该属性，也不允许修改该特性，默认为false
  })
  ```

  

#### 函数进阶

+ 定义函数

```js
//1.自定义（命名）
fn(){}
//2.函数表达式（匿名）
var fn = function (){}
//3.通过Function，由此可以看出，所有函数都是一个对象，他是Function的实例，所以他的隐士原型指向的Function的原型对象
var fn =new Function('参数1','参数2'，...,'函数体')
var fn = new Function ('console.log(23)')
var fn = new Function ('a','b','c','console.log(23)')
```

![1578463437](D:\project\ClassNotes\images\js\1578463437.png)

+ 调用函数

  ```js
  //1.普通函数
  function fn(){}
  fn()\fn.call()
  //2.对象的方法
  var o={
      say:function(){}
  }
  o.say();
  //3.构造函数
  function fn (){}
  new fn()
  //4.绑定事件函数
  btn.onclick=function(){}
  事件触发调用
  //5.定时器函数
  setInterval(function(){})
  计时调用
  //6.立即执行函数
  (function(){})()
  自动调用
  ```

+ this指向

```js
//1.普通函数
//this指向window
function fn(){}
fn()\fn.call()
//2.对象的方法
//this指向对象o
var o={
    say:function(){}
}
o.say();
//3.构造函数
// 构造函数和构造函数的原型对象this指向当前实例
function fn (){}
new fn()
//4.绑定事件函数
//this指向函数调用者，也就是btn
btn.onclick=function(){}
事件触发调用
//5.定时器函数
//this指向window
setInterval(function(){})
计时调用
//6.立即执行函数
//this指向window
(function(){})()
自动调用
```

![1578464404(1)](D:\project\ClassNotes\images\js\1578464404(1).png)

+ 改变函数的this指向

```js
//1.call(目标对象，参数1，参数2)
var 0={
    name:'123'
}
function fn(){}
fn.call(0)
    // call主要用于继承
	function F(name){
        this.name = name;
    }
	function S(name){
        F.call(S,name) 
    }



//2.apply(目标对象，[参数1，参数2])
fn.apply(0)
   // apply主要辅助于使用数学内置对象
    var arr =[1,3,99,66,3]//注意：如果数组里面是数值型的，那么apply获取的arr就是数组，字符串类型的，那么apply就是一个个字符串
    var max = Math.max.apply(null,arr);
    //由此得出这个数组中的最大值



//3.bind(目标对象，参数1，参数2)
//不会自动执行，，而是返回一个原函数改变this后的拷贝，所以需要用东西来接收

//如果遇到需要多个this指向，然后就可以通过后面参数的形式来实现
btn.onclick = function (){
    this.dsiable =true;//这个this指向的是btn
    setTimeOut(function(){
        this.disable =false;//这个this在定时器内部原本指向的window，但是后面被改变了this的指向
    }.bind(this),3000)//这个this是在定时器的外面，但是在定时器的里面所以指向的是btn
}


```

+ 严格模式

什么是严格模式？

![1578559913(1)](D:\project\ClassNotes\images\js\1578559913(1).png)

在js里面第一行加上’use strict‘,这样下面的js代码就会按照严格模式执行

也可以单独给函数设置严格模式

> `规范`
>
> 1. 变量需要先声明后使用
> 2. 严禁删除已经声明过的变量，比如使用delete x;
> 3. 全局作用域中函数中的this指向undefined
> 4. 构造函数不加new来调用，里面的this就会报错
> 5. 实例化的this还是指向实例；定时器this还是指向window；事件还是指向调用者
> 6.  函数里面不允许有重名的变量
> 7. 不允许在非函数代码块内申明函数，比如if、for里面
>
> 



+ 高阶函数

接收函数作为参数或将函数作为返回值输出

```JS
function fn (a,b,callback){
    consloe.log(a+b);
    callback && callback();
}
fn(1,2,function (){
    console.log('end')
})
```



+ 闭包

> `拓展：`
>
> 变量根据作用域的不同分为两种：全局和局部
>
> 1. 函数内部可以使用全局变量
>
> 2. 函数外部不能使用局部变量
> 3. 当函数执行完毕，本作用域内的局部变量会销毁

> `闭包`：
>
> 指有权访问另一个函数作用域中的变量的函数
>
> 就是一个作用域可以访问另一个函数内部的局部变量

> `闭包的作用：`
>
> 延伸了变量的作用域范围

```js
/*1. 我们利用动态添加属性的方式*/
for(var i = 0; i<lis.length; i++){
    lis[i].index = i;
    lis[i].onclick = function(){
        console.log(this.index);
    }
}

/*2. 利用闭包的方式*/
for(var i = 0; i<lis.length; i++){
    // 利用for循环创建了4个立即执行函数
    (function(i){
         lis[i].onclick = function(){
        console.log(this.index);
    }
    })(i)
}
```

```js
//1. 没有闭包产生示例
var name = 'this window';
var obj = {
    name:'my obj',
    getFn:function(){
        return function(){
            return this.name;
        }
    }
}
obj.getFn()();
//------------解析-------------------
1. 首先看obj.getFn()
var f = obj.getFn();
类似于
var f= function(){
    return this.name;
}
2. 第二个（）表示
f（）
类似于
function(){
    return this.name;
}()
看着就像立即执行函数，所以里面的this指向的是window

//2.有闭包的示例分析
var name = 'this window';
var obj = {
    name:'my obj',
    getFn:function(){
        var that = this;
        return function(){
            return that.name;
        }
    }
}
obj.getFn()();
//------------解析-------------------
1. 首先看obj.getFn()
首先会执行var that = this;当前this指向的是谁调用它就指向谁，所以是obj
然后就是
var f = obj.getFn();
类似于
var f= function(){
    return that.name;
}
这个that是函数外的局部变量，所以是指向的是obj,所以获取obj.name
2. 第二个（）表示
f（）
类似于
function(){
    return that.name;
}()
看着就像立即执行函数，里面that指向的是obj，所以是my obj
```

```js
//3.闭包应用
function fn(msg,time){
    setTimeOut(function(){
        alert(msg)//这里使用了外部函数的变量msg,所以构成了闭包
    },time)
}
fn('msg',2000);

```





+ 递归

如果一个函数在内部可以调用其本身，那么这个函数就是递归函数

递归函数的作用和循环的作用相似

> 使用递归的时候为了防止栈溢出，一定要加一个特殊条件return退出

```js
var data=[{
    id:'1',
    goods:[{
        id:'11'
    }]
}]
//我们想要做输入ID号，就可以返回的数据对象
//利用foreach去遍历每一个对象
function fn(json,id) {
    var 0 = null;
    json.foreach(function (item){
        if(item.id === id){
            o = item;
        }else if(item.goods &&  item.goods.length>0){
            o = fn(item.goods, id)
        }
    })
    return o;
}


fn(data, 11);
```





+ 深拷贝、浅拷贝

> 浅拷贝只是拷贝一层，更深层次对象级别的只拷贝引用
>
> 深拷贝拷贝多层，每一级别的数据都会拷贝

```js
var obj={
    name:'123',
    ma:{
        id:23
    }
}

var o={};
//浅拷贝1
for(var key in obj){
    o[key] = obj[key]
}
//浅拷贝2
Object.assign(o, obj)
```

```js
//深拷贝
function deepcopy(newobj, oldobj){
    for(var k in oldobj){
        //判断我们属性值属于那种类型
        var item = oldobj[k];
        if(item instanceof Array){
            //是数组类型就先创建一个空数组
            newobj[k] = [];
            deepcopy(newobj[k], item)
        }else if(item instanceof Object){
            //是对象类型就先创建一个空对象
            newobj[k] = {};
            deepclone(newobj[k], item)
        }else{
            //简易类型，就直接赋值
            newobj[k] = item;
        }
    }
}
```













