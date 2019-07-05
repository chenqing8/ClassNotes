 # 在项目中使用框架
1. **运行**：

   ```
   npm i react react-dom -S
   ```
   安装包
   `-S`:表示从上线到开发都需要这个包
   `-D`：表示工具就放在这里面
   `react`:是提供创建组件和虚拟DOM、生命周期的包
   `react-dom`:专门进行dom操作把代码放在页面中，主要运用的场景就是reactDOM.render();
2. **导入**
    ```
    import React from "react"
    import ReactDOM from "react-dom"
    ```
    这两个导入时候的成员名必须这样写，大小写不能变
3. **创建虚拟dom元素（至少三个参数**
   参数1：创建的元素类型，字符串，表示元素的名字
   参数2：是一个对象或者null表示当前这个DOM的属性
   参数3：表示子节点#
   参数n:表示其他子节点,和参数三是平行的子节点
   ```
   const my = React.createElement('h1',null,'这是h1','此处可以放react创建的其他虚拟dom，相当于是嵌套')
   const my = <h1>这是h1 此处可以放react创建的其他虚拟dom，相当于是嵌套</h1>
   ```
   `my`:接收了这个dom，但是只在内存中，需要渲染到页面上
   在js文件中不能使用html标记语言，如果想要使用的话就需要用`babel`进行转换为上面的js写法
   >`注意：`在js中混合使用html的语法叫做`jsx`,是符合`xml`规范的js,所以jsx本质就是使用React.createElement转换为dom 然后显示
4. **渲染dom到页面**
   参数1：要渲染到哪个虚拟dom元素上面
   参数2：指定页面上的容器，好存放dom
   ```
   reactDOM.render(my,document.getElementById('app'));
   ```
   `document.getElementById('app')`:不能使用'#app',因为webpack打包后把代码嵌入到html中，此时你想使用html的元素就应该这样使用，才能获取到
5. **如何使用jsx**
   ```
   npm i babel-core babel-loader babel-plugin-transform-runtime babel-preset-env babel-preset-stage-0 babel-preset-react -D
   ```
   安装好后：在`.babelrc`中配置
   ```
   {
       "preset":["env","stage-0","react"],
       "plugin":["transform-runtime"]
   }
   ```
   在webpack中设loader
   `webpack默认只能处理.js的文件，所以其他的文件，都需要配置第三方插件`
6. **jsx语法**
    >把js代码放在{}中，当你要在jsx里面写一些js表达式的时候就需要使用{}包裹起来

    >在jsx中循环数组到dom中：
     在react中key需要放在forEach或map直接控制的元素上面

    >`方案一：在html外循环`
     ```
     const arr=[1,2,3,4];
     const arr1=[];
     arr.forEach(item=>{
         let item=<span key={item}>{item}</span>;
         arr1.push(item);
     })

     const my=<div><ul>{arr1}</ul></div>
     ```
    >`方案二：在html内循环`
    ```
    const arr=[1,2,3,4];
    const my=
    <div>
      <ul>
        {arr.map(item=>{
            return <span key={item}>{item}</span>
        })}
      </ul>
    </div>
    ```
    >`注意：forEach和map功能一样，唯一的区别就是map是直接操作每项后返回新的数组，forEach没有返回的功能，需要手动push到一个数组中`
7. **jsx注释**
    >注释必须用{}来注释：
    *多行注释：{/**/}
    *单行注释：{//}
8. **在jsx中添加class名**
    >需要用className来代替class，其中htmlFor替换label的for属性
9. **jsx的注意事项**
10. **创建组件的方式**   
    >`方案一：组件首字母必须大写`
      ```
    function Hello(){
      return <div>这是hello组件</div>
    }
    ReactDOM.render(<div>
    <Hello></Hello>
    </div>,document.getelementById("app"))
    ```
    > `方案二：从外部引入子组件`
     ```
     * 默认情况下，不做其他配置，引入的文件后缀是不能省略的，会报错，如果想省略，在webpack中添加  resolve:{extensions:['.js','.jsx','.json']}
     import Hello form '../Hello.jsx'
     ```
     ```
     * 子组件中必须导入react，因为这个文件使用的react来创建组件的
     import React from "react"
     export defalut function Hello(){
       return <div>123</div>
     }
     ```
    >`方案三：用class关键字，继承自React.Component`
     ```
     `当你使用import React,{compontent} form 'react';component等同于React.Component`
     class Hello extends React.Component {
       constructor(){
         super();
         this.state={};//相当于vue中的data(){return{}},所以我们可以在这里面定义属性，使用就直接this.static.名字
       }
       * 必须要有一个render函数，并且要返回一个合法的jsx虚拟dom结构
       render(){
         * render必须有返回值
         return null;
       }
     }
     ```
    > `function和class的区别`
     *用class创建的组件有自己的私有数据和生命周期和props，使用function就只有props，没有私有数据和生命周期
     *
    >`无状态组件`
    没有state状态，数据都是从仓库来的，react-redux的props来的，所以直接传值返回即可
    `能使用无状态组件就使用，因为它比class的组件性能好些`

    ```javascript
    const Header=(props)=>{
       return <HeaderWrapper>
                    <NavSearch className={props.focuse ? "focused" : ""} onFocus={() => props.handleFocuse()} onBlur={() => props.handleBlur()}>
                    </NavSearch>
      </HeaderWrapper>
    }
    ```
11. **组件传值**   
    无论是vue还是react，props的值都是只读的不能被重新赋值
    >`方案一：格式（子组件接收的名字={父组件传的值}）,然后子组件用形参接收即可，如props`
    `name={MyName.name} age={MyName.age}和{...MyName}效果一样`
    ```
    function Hello(props){
      return <div>这是hello组件--{props.name}</div>
    }
    const MyName={
      name："lnyy",
      age:23
      }
    ReactDOM.render(<div>
    <Hello name={MyName.name} age={MyName.age}></Hello>
    </div>,document.getelementById("app"))
    ```
    >`方案二：`
    ```
    const MyName={
      name："lnyy",
      age:23
      }
    ReactDOM.render(<div>
    <Hello name={MyName.name} age={MyName.age}></Hello>
    </div>,document.getelementById("app"))
    
    class Hello extends React.Component {
       render(){
         * 使用class创建的组件，传递过来的props，不需要使用参数接收，可以直接this.props.名字
         return {this.props.name};
       }
     }
    ```
12. **class的使用**
    >`注1：在class里面只能写构造器、静态方法、静态属性、实例方法`

    >`注2：class内部还是用function普通的构造函数来实现的`

     ```
     * 默认情况下，class里面有一个默认的空的构造器
     class Hello{
       /*构造器的作用就是每当你new的时候，必然会优先执行构造器中的代码  */
       constructor(name，age){
         this.name=name;
         this.age=age; 
       }，
       static info='111';
     }
     const h=new Hello('aa',12);
     通过new出来的实例访问的属性叫做实例属性；
     通过构造函数直接访问到的属性叫做静态属性=>{
       在` function `中，直接用function名.静态属性；
       在` class `中，是用static申明的静态属性；
     }；
     实例方法=>{
       在` function `中，直接用function名.prototype.方法；
       在` class `中，直接写方法()；
     }
     静态方法：=>{
       在` function `中，直接用function名.方法；
       在` class `中，直接用static声明；
     }
     ```
13. **class继承**
    >`可以使用父类的实例属性和实例方法`
    class 类名 extends 父类{}
    在子类中定义一个单独的构造器一定要在里面使用super(),当你不写构造器的时候，默认就是空的构造器里面有一个super(); 
    在子类构造器中实例属性只能放在super()后面   

    >`问题一：为什么一定要调用super()`
    如果一个子类通过extends继承父类，那么在子类的构造函数中，必须手动优先调用一下父类的构造函数，再执行子类的构造器

    >`问题二：什么是super`
    是一个函数，而且他是父类的构造器，子类中的super，其实就是父类中构造器的引用

    >`问题二：调用super()后，父类的实例属性显示undefined`
    因为super()代表父类构造器，如果父类的构造器有参数，你不传那就是undefined,
14. **style行内样式**
     >在jsx中如果想使用行内样式，不能为style设置字符串=>
     而是应该`style={{color:'red',fontSize:'10px'}}`=>
     外层花括号表示我要写js了,里层表示要写对象了可以抽离出去,注意要使用驼峰法
     在行内样式中，如果是数字类型的则不用单引号，字符类型的就需要单引号
15. **className美化样式**
      >`导入样式表`
      ```
      import css form "remote.css";//这个地方需要webpack进行css处理
      <div className="title"></div>
      ```
      >`问题1：这个样式表只在该组件中生效吗？`
      import接收到的css是一个空对象，应为css中不能写export对css进行暴露
      默认情况下，在组件中用import导入的样式表是在整个项目中生效的，因为样式表没有模块的作用域，但是可以添加。

      >`给样式表添加作用域`
      给css后缀改为js
      or
      在webpack配置css-loader后面加一个？modules就可以了

      >`注意:`css模块化只对类和id选择器有效，对标签选择器无效
      ```
      {test:/\.css/,use:['style-loader','css-loader?modules']}
      ```
      然后，使用方式：
      ```
      className={css.类名}
      id={css.id名}
      ```
      >`多个样式`
      className={css.类名+" "+css.类名}
      className={[css.类名,css.类名].join(" ")}
16. **自定义生成的样式名规则**
    需要在webpack的css中配置localIdentName
    >localIdentName有几个参数：
     `path`:表示样式表相对于项目根目录所在路径,结尾自带`-`；
     `name`:表示样式表文件的名称,结尾不带`-`；
     `local`:表示样式的类名定义名称,结尾不带`-`；
     `hash:length`:表示32位的hash值,防止类名重复，一般取5位就不会重复了。
     ```
    {test:/\.css/,use:['style-loader','css-loader?modules&localIdentName=[path][name]-[local]-[hash:5]']}
     ```
17. **解决样式模块化后,生成模块化名字**   
    >目的:想要保持原本定义时候的class名
    `:global(类名)`：被她包裹起来的类名，不会被模块化，而是会全局生效
    `:local(类名)`：表示被模块化，一般不用写，因为这是默认情况，所以很少用
18. **第三方样式表**
    >第三方样式表中包含字体图标，所以需要配置webpack:
    ```
    自己网上查吧
    ```
    ```
    在原来直接引用样式名的基础上，需要通过导入的对象点出来
    import boocss form "bootstrap/dist/index.css";
    className={boocss.btn}
    className={[boocss.btn,boocss['btn-waring']].join(" ")}
    ```
    >一般自己写的样式需要启用模块化，但是第三方样式就不需要
    `解决办法1:`
    自己写的样式以`.less`或者`.scss`结尾
    你只需要给less或者scss设置模块化就行了，
    然后
    第三方样式就可以直接使用了
    className="btn"
19. **事件绑定**
    ```javascript
      class Toggle extends React.Component {
        constructor(props) {
          super(props);
          this.state = {};
          // 这个绑定是必要的，使`this`在回调中起作用
          this.handleClick = this.handleClick.bind(this);
        }
 
        handleClick() {
          this.setState(prevState => ({
            isToggleOn: !prevState.isToggleOn
          }));
        }
 
        render() {//当组件加载的时候render就被调用了，相当于vue中的mounted
          return (
            * 在react中有一套自己的事件机制，事件名需要是小驼峰命名，onClick={里面是js代码}
            * 调用的方法 不要加（），会被立即执行
            * onClick只接受function，不接受方法调用
            * 箭头函数本身就是一个匿名的function函数
            * 如果箭头函数后面只有一句就可以省略里面的{}
            <button onClick={this.handleClick}>
            </button>
            <button onClick={function(){console.log(123)}}>
            </button>
            <button onClick={()=>console.log(123)}>
            </button>
            /*  ##===============这是最标准的，react推荐使用的事件绑定事件===============## */
            <button onClick={()=>this.show(123)}>
            </button>
          );
        }
        show=(num)=>{
          alert(num)
        }
      }
 
      ReactDOM.render(
        <Toggle />,
        document.getElementById('root')
      );
    ```
20. **改变state的值**
    在react中推荐使用`setState`更改属性，如果使用`this.state.属性=新值`，只会改变state，但是页面数据却没有实时更新
    在setState中只会把对应的属性进行更新，所以不会对state进行整体替换
   
    ```
    this.setState({需要改变的属性名：新的值})
    ```

    `setState`是异步操作的，所以想要取到更新后的值，必须在`setState`回调函数中使用

    ```
     this.setState({需要改变的属性名：新的值},function(){
      alert( this.state.需要改变的属性名)
     })
    ```
    `推荐写法：`
    ```
    this.setState((pre)=>{<!-- pre相当于this.state -->
      pre.属性名：新的值
    })
    ```
21. **数据双向绑定**
    *默认情况下，在react中，如果页面元素绑定了state的状态值，那么，每当state状态值变化必然会自动把状态值同步到页面上。（单项数据流）
    *如果UI界面上文本框内容变化了，如果想把最新的值同步到state中去，此时react没有自动更新的功能。
    `解决办法：`在react中需要程序员手动监听文本框的onChange，然后再拿到onChange最新的文本框的值，手动this.setState({})手动跟新到state中
    >input
    如果我们只是把value绑定了state状态，但是不适用onChange的话，这个value就是只读的
    当input值发生改变的时候必然会触发onChange事件
    
    >获取onChange的值：
    `方案一：通过事件参数e`
    ```
    <input value={this.state.msg} onChange={(e)=>this.textChange}>

    textChange=(e)=>{
      this.setState({msg:e.target.value});
    }
    ```
    `方案二：ref的引用：this.ref.引用名字`
       ```
    <input value={this.state.msg} onChange={()=>this.textChange} ref="text">

    textChange=()=>{
      this.setState({msg:this.ref.text.value});
    }
    ```
22. **生命周期**
     生命周期：每个组件从创建到运行到销毁，这一个过程，在这个过程中触发的函数就是生命周期函数
23. **redux react-redux**
    >npm install redux react-redux

    ```javascript
    <!-- APP.js -->
      `Provider`表示给Header组件提供store值
      import {Provider} from "react-redux"
      import {store} from  "./store"
      function App() {
        return (
          <Provider store={store}>
            <Header />
          </Provider>
        );
      }
    ``` 
    ```javascript
    <!-- header.js -->
      import {connect} from "react-redux"
      import {BLURACTION,FOCUSEACTION} from "../../store/actionCreate"
      const Header=(props)=>{
        /* 可以优化 const {handleFocuse，focuse，handleBlur}  然后就不用props.了*/
      return (<HeaderWrapper>
                    // <NavSearch className={focuse ? "focused" : ""} onFocus={() => handleFocuse()} onBlur={() => handleBlur()}> 
                    <NavSearch className={props.focuse ? "focused" : ""} onFocus={() => props.handleFocuse()} onBlur={() => props.handleBlur()}>
                    </NavSearch>
      </HeaderWrapper>)
      }
      const MapStateToProps=(state)=>{
          return{
              focuse:state.focuse
          }
      }
      const MapDispatchToProps=(dispatch)=>{
          return{
              handleFocuse:()=>{
                  dispatch(FOCUSEACTION())
              },
              handleBlur:()=>{
                  dispatch(BLURACTION())
              }
          }
      }
      export default connect(MapStateToProps,MapDispatchToProps)(Header)
    ```
    ```javascript
    <!-- reduce.js -->
      import {FOCUSE,BLUR} from "./ationType"
      const defaultState={
          focuse:false
      }
      export const reducer=(state=defaultState,action)=>{
          if(action.type===FOCUSE){
              return{
                  focuse:true
              }
          }
          if(action.type===BLUR){
              return{
                  focuse:false
              }
          }
          return state;
      }
    ```
    ```javascript
    <!-- ationType.js -->
      export const FOCUSE="focuse"
      export const BLUR="blur"
    ```
    ```javascript
    <!-- actionCreate.js -->
      import {FOCUSE,BLUR} from "./ationType"
      export const BLURACTION=()=>{
          return {
              type:BLUR
          }
      }
      export const FOCUSEACTION=()=>{
          return {
              type:FOCUSE
          }
      }
    ```
    >管理多个reducer`combineReducers` 

    ```javascript

    ```
    >`immutable.js`:为了防止原始state被修改,toJS()

    >npm install immutable

    ```javascript
    <!-- reduce.js -->
      import {FOCUSE,BLUR} from "./ationType"
      import {fromJS} from "immutable"//state在哪里，就在那里引入immutable。可以吧js对象转化为immutable对象
      /* .set和.merge的作用是一样的 */
     /*  const defaultState={
          focuse:false
      } */
      const defaultState=fromJS({
          focuse:false
      })
      export const reducer=(state=defaultState,action)=>{
          if(action.type===FOCUSE){
              /* return{
                  focuse:true
              } */
              return state.set("focuse",true)
          }
          if(action.type===BLUR){
              /* return{
                  focuse:false
              }  */
              /* return state.set("focuse",false) */
              return state.merge({
                focuse:false
              })
          }
          return state;
      }
    ```
     ```javascript
    <!-- header.js -->
      import {connect} from "react-redux"
      import {BLURACTION,FOCUSEACTION} from "../../store/actionCreate"
      const Header=(props)=>{
      return <HeaderWrapper>
                    <NavSearch className={props.focuse ? "focused" : ""} onFocus={() => props.handleFocuse()} onBlur={() => props.handleBlur()}>
                    </NavSearch>
      </HeaderWrapper>
      }
      const MapStateToProps=(state)=>{
          return{
              /* focuse:state.focuse */
              focuse:state.get("focuse")
          }
      }
      const MapDispatchToProps=(dispatch)=>{
          return{
              handleFocuse:()=>{
                  dispatch(FOCUSEACTION())
              },
              handleBlur:()=>{
                  dispatch(BLURACTION())
              }
          }
      }
      export default connect(MapStateToProps,MapDispatchToProps)(Header)/* 如果只是获取数据,而不用改变state数据的话,可以写null,也可以省略第二个参数 */
    ```

    >`redux-immutable`统一数据格式

    npm install redux-immutable

24. **react路由**
    npm install react-router-dom

    ```js
    /*APP.js*/
    import {BrowserHistory,Route} from "react-router-dom"
    calss APP extends React.Component{
      render(){
        return (
          /* 这里面只能有一个子元素 */
          <div>
            <Header/>
            <BrowserHistory>
              <div>/*  BrowserHistory 里面只能有一个子节点 component：表示使用组件*/
                <Route path="/" exact render={()=><div>header</div>}></Route>
                <Route path="/detial" exact render={()=><div>detial</div>}></Route>
                <Route path="/detial" exact component={Home}></Route>
              </div>
            </BrowserHistory>
          </div>
        )
      }
    }
    ```
25. **动画 react-transition-group**
    ```javascript
      npm install react-transition-group
      import { CSSTransition } from 'react-transition-group'    
    ```
    ```html
       <!-- 想要给谁添加动画就给谁包裹起来,目前是input，in：表示是否开始动画，是布尔值；timeout：表示动画时长；classNames：表示动画名字 --> 
       <div className="a">
      <CSSTransition in={this.state.focuse} timeout={200} classNames="slide">
        <input />
      </CSSTransition>
      </div>
    ```
    ```css
    //在input的外层添加动画名字的动画，目前是slide，所以要添加,slide-enter,slide-enter-active,slide-leave,slide-leave-active
    .a{
      .slide-enter{
        width:100px;
        transition:all .2s ease-out;
      }
      .slide-enter-active{
        width:200px;
      }
      .slide-leave{
       width:200px;
        transition:all .2s ease-out;
      }
      .slide-leave-active{
        width:100px;
      }
    }
    //或者在input里面添加动画
    input{
      width:160px;
      	&.slide-enter {
	      	transition: all .2s ease-out;
        }
        &.slide-enter-active {
          width: 240px;
        }
        &.slide-exit {
          transition: all .2s ease-out;
        }
        &.slide-exit-active {
          width: 160px;
        }
    }
    ```
26. **styled-components**
    就相当于一个一个的组件，首写字母必须大写  
    ```javascript
    npm install styled-components

    <!-- style.js -->
    import styled from 'styled-components';
    export const Header = styled.div`
    &:left{/* div下面的子类 */
      float:left;
    }
    width:100px;
    height:100px;
    color:red;
    background:url(${img});/* `${}`需要使用字符串的时候使用 */
    `
    export const Input = styled.input.attrs({/* `attrs`标签需要添加属性的时候就可以用这个属性 */
        placeholder:'搜索'
    })`
    width:100px;
    height:100px;
    &::placeholder{/* 修改属性样式方法 */
      color:red
    }
    `
    <!-- App.js组件 -->
    import {Header,Input} from  "./style"
    <!-- 相当于hello被一个div包裹起来了 -->
    <Header>
      <div>hello</div>
      <div className='left'></div>
    </Header>
    <Input></Input>
    ```
27. **组件值传递**
    ```javascript
    /* =====父传子====== */
     /* 父 给子组件传tableList数据*/
      <Child list={this.state.tableList}></Child>
     /* 子 获取父组件传来的tableList数据*/
      this.props.list
    /* =======子传父==========因为react是单项数据流，只准父组件向子组件传值，子组件不能修改，唯一就是调用父组件方法，让父组件来修改 */
     /* 父 给子组件传修改tableList数据的方法*/
      <Child cahngeList={this.cahngeList}></Child>
     /* 子 获取父组件传来的方法，并改变数据集*/
      this.props.cahngeList()
    ```
28. **this指向问题**
    `方案一:不推荐，影响性能`
    ```
    render(){
      <item onCLick={this.changeItem.bind(this)}></item>
    }
    ```
    `方案二:`
    ```
    construction(){
      this.changeItem:this.changeItem.bind(this)
    }
    render(){
      <item onCLick={this.changeItem}></item>
    }
    ```
    `方案三:`
    ```
    changeItem=()=>{

    }
    render(){
      <item onCLick={()=>this.changeItem()}></item>
    }
    ```
#遇到的问题
1. 引入组件必须是大写字母开头
   ```javascript
    import Header from './header/header'
   ```
2. `npm run eject`报错解决办法：
    ```
    git init 
    git add .
    git commit -m "12"
    npm run eject
    ``` 
3. 16.8react使用less限制作用域
   ```JavaScript
    <!-- 安装 -->
    npm install less less-loader -D

    <!-- 在webpack中添加less的规则 -->
    {
              test: /\.less$/,
              exclude: /node_modules\.less/,
              use: [
                require.resolve('style-loader'),
                {
                  loader: require.resolve('css-loader'),
                  options: {
                    importLoaders: 1,
                    modules: true,
                  },
                },
                {
                  loader: require.resolve('less-loader'), // compiles Less to LESS
                  options: {
                    importLoaders: 2,
                    modules: true,
                    getLocalIdent: getCSSModuleLocalIdent,
                  },
                },
              ],
            },
    <!-- 在页面中引用方式 -->
    import a from  "./a.less"
    className={a.a}
   ```
4. 事件绑定的时候方法要加()
   ```javascript
    onFocus={()=>this.handleFocuse()}

     handleFocuse=()=>{
        console.log(123);
        this.setState({
            focuse:true
        })
    }
   ```
5. `export const a`导出的东西，应该使用`{a}`来导入
   在一个模块中，`export default` 只允许向外暴露一次
   可以同时使用`export default` 和`export` 向外暴露成员
   使用`export`向外暴露的成员，只能使用`{ 名字与导出一致 }`的形式来接收，这种形式，叫做【按需导出】
   `export default`可以其他名字来接收