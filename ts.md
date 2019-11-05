<<<<<<< HEAD
## vue-property-decorator的用法
这个组件完全依赖于vue-class-component.它具备以下几个属性:

`@Component` (完全继承于vue-class-component):相当于原来的Component,现在放在了class前面
`@Emit`:@Emit()不传参数,那么它触发的事件名就是它所修饰的函数名。@Emit(name: string),里面传递一个字符串,该字符串为要触发的事件名.
@Inject
@Provice
@Prop
`@Watch`：@Watch装饰器来替换Vue中的watch属性,以此来监听值的变化.
@Model
Mixins (在vue-class-component中定义);
```ts
// 父组件
<HelloWorld msg="Welcome to Your Vue.js + TypeScript App" @emitTodo="bb"/>

// 子组件
<h1 @click='emitTodo("hahah")'>{{text}}{{ msg }}{{count}}</h1>
<script lang="ts">
    import {Vue, Component,Emit,Watch,Prop,Model} from 'vue-property-decorator';
    import Hello from '../Hello.vue';
    import { State, Action, Getter} from "vuex-class";
    // 相当于原来的Component,这个是必须写的
    @Component({
        components{
            Hello
        }
    })
    export default class "组件名" extends Vue{
        // 相当于原来的data()
        ValA: string = "hello world";
        ValB: number = 1;
        // 需要用get关键字,相当于原来的计算属性computed
        get ValA(){
            return 1;
        }
        // 执行emitTodo后执行@Emit括号中的方法
        @Emit('reset')
        // 如果只有一个方法需要向上传值，@emit（）可以放在任一位置，否则必须放在对应的方法上面
        emitTodo(n: string){//触发该方法,执行然后会在父组件找有没有相应的驼峰方法，如果想要传递值给父组件，就写一个形参
        }
        //@Watch使用非常简单,接受第一个参数为要监听的属性名 第二个属性为可选对象.@Watch所装饰的函数即监听到属性变化之后的操作.
        @Watch('child')//child是检测的数据,下面就是检测后执行的方法
        onChangeValue(newVal: string, oldVal: string){}
        @Provide()//在父组件中申明Provide
        foo = 'foo'
        @Inject()//在子组件中接受Inject
        foo!: string;
        //propA: {
        //     type: Number
        // },
        // propB: {
        //     default: 'default value'
        // },
        // propC: {
        //     type: [String, Boolean]
        // },
        // 这里 !和可选参数?是相反的, !告诉TypeScript我这里一定有值.
        // @Prop接受一个参数可以是类型变量或者对象或者数组.@Prop接受的类型比如Number是JavaScript的类型,之后定义的属性类型则是TypeScript的类型.
        @Prop(Number) propA!: number;
        @Prop({default: 'default value'}) propB!: string;
        @propC([String, Boolean]) propC: string | boolean;
        // @Model()接收两个参数, 第一个是event值, 第二个是prop的类型说明, 与@Prop类似, 这里的类型要用JS的. 后面在接着是prop和在TS下的类型说明.
        @Model ('change', {type: Boolean})  checked!: boolean;
        // mapstate\mapaction\mapgetter
        @State login: boolean;
        @Action initAjax: () => void;
        @Getter load: boolean;
        //methods
        add(a:number):number{
            console.log('123')
        }
        //生命周期
        mounted(){
            console.log('13')
        }
    }
</script>
```

在使用Vue进行开发时我们经常要用到混合,结合TypeScript之后我们有两种mixins的方法.

一种是vue-class-component提供的.
```ts
//定义要混合的类 mixins.ts
import Vue from 'vue';
import  Component  from 'vue-class-component';

@Component  // 一定要用Component修饰
export default class myMixins extends Vue {
    value: string = "Hello"
}
// 引入
import  Component  {mixins}  from 'vue-class-component';
import myMixins from 'mixins.ts';

@Component
export class myComponent extends mixins(myMixins) {
                          // 直接extends myMinxins 也可以正常运行
      created(){
          console.log(this.value) // => Hello
    }
}
```
第二种方式是在@Component中混入.

我们改造一下mixins.ts,定义vue/type/vue模块,实现Vue接口
```ts
// mixins.ts
import { Vue, Component } from 'vue-property-decorator';


declare module 'vue/types/vue' {
    interface Vue {
        value: string;
    }
}

@Component
export default class myMixins extends Vue {
    value: string = 'Hello'
}
```
混入
```ts
import { Vue, Component, Prop } from 'vue-property-decorator';
import myMixins from '@static/js/mixins';

@Component({
    mixins: [myMixins]
})
export default class myComponent extends Vue{
    created(){
        console.log(this.value) // => Hello
    }
}
```
总结: 两种方式不同的是在定义mixins时如果没有定义vue/type/vue模块, 那么在混入的时候就要继承该mixins; 如果定义vue/type/vue模块,在混入时可以在@Component中mixins直接混入
=======
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









>>>>>>> a90c8163dcb709798289cad873e9c9e31b8902ee
