#js简史
####js的组成部分
+ 核心：描述该语言该的语法和基本对象
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






