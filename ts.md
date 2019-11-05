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