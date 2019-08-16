# ts
1. **参数类型**
 ``` js
var myname:string = "zhang san"/* 此时就不能给myname赋其他类型的值 */
var ax="123"/* 此时ax也只能给ax赋值字符串类型的，ts中虽然不指定类型，默认第一个赋值的类型就是他的类型 */
function a():void{/* 申明这个方法不需要返回值，如果有就会报错 */

}
function a():string {/* 申明这个方法返回一个字符串的返回值 */

}
function a(name:string):string {/* 参数必须是字符串类型的，然后返回一个字符串的值 */

}
 ```
2. **参数默认值**
```js
function a(name:string,age:number):string {/* 参数必须是字符串类型的，然后返回一个字符串的值 */

}
a("",12)/* 必须有两个参数 */
function a(name:string,age:number=20):string {/* 参数必须是字符串类型的，然后返回一个字符串的值 */

}
a("")/* 有默认值就可以只写一个参数 */
```
3. **可选参数**
```js
function a(name?:string,age:number=20):string {/* 加个？表示可选参数，可选参数必选参数后面 */

}
a()
```
4. **函数新特性**
```js
function a(...args) {/* 可传任意数量的参数 */

}
a(1,2,3)/* args=[1,2,3] */
a(1,2,3,4)/* args=[1,2,3,4] */

//////////////////////////////////////////////

var args1=[1,2]
var args2=[1,2,3,4,5]
function a(a,b,c) {/* 可传任意数量的参数 */
console.log(a)
console.log(b)
console.log(c)
}
a(...args1)/* 1,2,undefined */
a(...args2)/* 1,2,3 */
```
5. **generator 函数**
可以控制函数的执行过程，手工暂停和恢复代码执行
```js
function* do(){
    consloe.log("start")
    yield;
    console.log("end")
}
var func1=do();
func1.next();/* 调用第一次的时候回碰到yield就停了下来，所以此时只打印出了start */
func1.next();/* 调用第二次的时候就能打印出了end了 */

```
6. **desstructuring析构表达式**
通过表达式将对象或者数组拆解成任意数量的变量
```js
/* 对象 */
function do(){
    return{
        code :"IBM",
        price:100,
        name:{
            name1:'12',
            name2:'23'
        }
    }
}
var {code,price} = do();/* {}里面的变量必须和return的字段一样，否则就会报错 */
var {code:codex,price} = do();/* 通过：起别名 */
var {code:codex,price,name:{name2}} = do();/* 获取变量 里面的变量*/

/* 数组 */
var ary=[1,2,3,4];
var [num1,num2]=ary;/* num1=1,num2=2 */
var [num1,,,num2]=ary;/* num1=1,num2=4 */
var [num1,num2,...other]=ary;/* num1=1,num2=2,other=[3,4] */

/* 方法 */
function do([num1,num2,...other]){
    return num1,num2,other;
}
do(ary)/* num1=1,num2=2,other=[3,4] */
```
7. **箭头函数**<!-- 没有什么特别的，和平时使用一样 -->
```js
var sum=(a1,a2)=>{
    return a1+a2;
}
var sum=a1=>{
    return a1;
}
var sum=()=>{
    return '123';
}
```
8. **表达式和循环**
```js
var ary=[1,2,3,4]
ary.des="forEach"
/* forEach会忽略掉循环的属性值并且不能break*/
ary.forEach(val=>consloe.log(value))/* 1,2,3,4 */
/* forin是循环的一个属性的名字或者下标 */
for(var i in ary){
    console.log(i)/*  0,1,2,3,des */
}
/* forof是循环的是数组或者对象的值，可以break ,他会忽略掉ary的属性*/
for(var i of ary){
    console.log(i)/*  1,2,3,4 */
}
```
9. **面向对象的特性**
<!-- 类 -->
```js
class Per{/* 访问控制符，默认public，private:类的内部可以访问，protected：类的内部和子类可以访问 */
    constructor(name:string,public age:number){/* 没有给name属性添加访问控制符，就相当于没有设置name属性，在类里面就不能用this.name来访问 ,age就相当于在构造器外面有申明public age;,在类里面可以访问*/
        this.name=name;
    }
    // private name;
    private eat(){}
}
var p1=new Per("batman",12);
// p1.name="batman";
p1.eat();
/* 继承可以使用父类的属性和方法 */
class Stu extends Per{
    constructor(name:string,public age:number,id:string){
        super(name,age)
        this.id=id
    }
    code:string;
    study(){
        super.eat();
        this.dostudy();
    }
    private dostudy(){
        console.log("dostudy")
    }
}
var s1=new Stu("batman",12,"012");
s1.study()/* 不能让学生没有吃饭就开始工作，所以这就是访问控制符的用法 */
```
<!-- 泛型 -->
```js
class Stu extends Per{
    constructor(name:string,public age:number,id:string){
        super(name,age)
        this.id=id
    }
    code:string;
    study(){
        super.eat();
        this.dostudy();
    }
    private dostudy(){
        console.log("dostudy")
    }
}
var workers:Array<Per>=[];
workers[0]=new Per("zhangsan",12,"012")
workers[1]=new Stu("lisi",12,"012")
workers[2]=2/* 此时报错 ，因为我指定的workers是一个Per的数组类型*/
```
<!-- 接口 -->
```js
/* 1.接口声明属性 */
interface IPerson{
    name:string,
    age:number
}
class Person{
    constructor(public config:IPerson){/* 定义参数格式为接口格式 */

    }
}
var p1=new Person({name:"lisi",age:12})/* 传入的构造器必须是接口定义的格式 */


/* 2.关键字用法 */
interface Animal{
    eat();
}
class Sheep implements Animal{/* Sheep使用接口的时候必须实现接口中的方法 */
   eat(){

   }
}
```
<!-- 模块 -->
```js
就是export 和import的使用
```
<!-- 注解 -->

<!-- 使用已有的js工具包 -->
就是使用包带有`*.d.ts`的文件
在哪里找这样的文件呢，
`https://github.com/typings/typings`
然后里面有教程，下载你需要的文件，然后就直接使用就可以了









