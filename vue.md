# vueCli3.0

`注：`
1. **图形化界面**
    >在任意位置打开图形化界面：vue ui
        前提是要npm install -g @vue/cli

2. **vue.config.js**
    >修改打包后路径问题，新建一个vue.config.js(这个名字是官方推荐的)
        ```js
            /* vue.config.js 这个名字是vuecli3.0默认的*/
            module.exports={
                baseUrl:process.env.NODE_ENV==='production'
                ?'./'
                :'/'
            }
        ```
**less的使用：**
 npm install less less-loader -D
 然后在vue文件中直接使用即可<style lang="less" scoped>

3. **web字体引入**
    `方案一：`
        在index.html中，字体文件和index.html在同一个目录下
        <link rel="stylesheet" href="<%= BASE_URL %>web_font/daysOne.css">
        然后直接使用
        font-family: "Days One"
    `方案二：`
        在main.js中引入css,然后直接使用即可
        import '../public/web_font/daysOne.css'
    `方案三：`
    

4. **移动端rem的使用**
    ```js
    /* load:页面的html、css、js、图片等资源都已经加载完之后才会触发 load 事件
    DOMContentLoaded:HTML下载、解析完毕之后就触发
    */
    /* App.js */
        <script>
            export default {}
            document.addEventListener('DOMContentLoaded',()=>{
                const html=document.querySelector('html');
                let fontSize=window.innerWidth / 10;/* 根据屏幕宽度计算fontsize */
                fontSize=fontSize>50?50:fontSize;/* 这一步是为了当fontsize尺寸大于50的时候，就保持在50 */
                html.style.fontSize=fontSize+'px'/* 给html设置fontsize，然后页面根据它设置rem单位 */
            })
        </script>
        <style scoped>
            .App{
                font-size:1rem;
                font-family: "Days One"
            }
        </style>

    /* 正常时候我们麻烦计算，也不知道应该是多少rem */
        <script>
            export default {}
            document.addEventListener('DOMContentLoaded',()=>{
                const html=document.querySelector('html');
                let fontSize=window.innerWidth / 10;/* 根据屏幕宽度计算fontsize */
                fontSize=fontSize>50?50:fontSize;/* 这一步是为了当fontsize尺寸大于50的时候，就保持在50 */
                html.style.fontSize=fontSize+'px'/* 给html设置fontsize，然后页面根据它设置rem单位 */
            })
        </script>
        <style lang="less" scoped>
            @import url('../src/assets/css/gloab.less');
            .App{
                .px2rem(40);
                font-family: "Days One";
            }
        </style>
        /* gloab.less */
        @radio:375/10;/*  */
        .px2rem(@px){
            font-size: unit(@px/@radio,px);/* unit()没有第二个参数就是就是去单位 ，带有第二个参数表示加单位*/
        }
        /* 现在有了一个更方便的东西就是px2rem-loader,直接在vue.config.js中配置后，app.vue中设置了html的fontsize后就可以了 */
        /* vue.config.js */
        module.exports = {
            chainWebpack: config => {
                config.module
                .rule('scss')
                .oneOf('vue')
                .use('px2rem-loader')
                .loader('px2rem-loader')
                .before('postcss-loader') // this makes it work.
                .options({ remUnit: 37.5})/* remUnit是你拿到的设计稿的宽度/10 */
                .end()
            }
        }
        /* 然后直接正常开发就可以直接编译为rem了 */
    ```


5. **vuex**
  系统学习后
  可以看看vue微信读书3.4-4.2.mp4（有点绕，没大懂）
  import store from './store/index'必须小写
  vuex是由

6. **ngnix**
    在官网上面下载安装包，在包目录下进入cmd，运行start nginx.exe会有东西闪退
    没关系，我们在浏览器中输入localhost，会出现welcome
    然后当你想新建一个资源服务器，打开包目录下面的conf文件夹中的nginx.conf文件，
    ```js
    server {
        listen       81;/* 这个就是在浏览器中输入的端口 */
        server_name  localhost;/* 这个就是在浏览器中输入的地址 */
        autoindex on; /* 是否允许访问目录 */
        location / {/* add_header这四句话是用来解决页面访问nginx静态资源服务器的时候，出现的跨域问题 */
            add_header 'Access-Control-Allow-Origin' '*';
            add_header 'Access-Control-Allow-Credentials' 'true';
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
            add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
            root   D:/WebProjects/resource;/* root后面就是资源服务器的文件地址，就是你想作为服务器新建的文件夹 ，注意斜杠的方向*/
        }
    }
    ```
    然后在cmd中执行nginx -t出现了successful就代表配置文件修改成功，
    然后在cmd中执行nginx -s reload 重启nginx服务
    然后你在浏览器中输入localhost:81就会成功了，就有拉本地资源服务器了。
    在页面中请求本地服务器接口地址就是http://本地ip:81/

7. transition 过度动画的样式 要与v-show的div同级

8. **minxin**
    用来复用代码，可以是方法，
   ```js
    /* 新建一个mixin文件 */
    import {mapGetters} from 'vuex'
    export const ebookMixin={
        computed: {
            ...mapGetters(['filename','filename1'])
        },
        methods: {
            ...mapActions(["setShowMenu","setFileName"])
        },
    }
    /* 在需要使用vuex中的地方 */
    import {ebookMixin} from 'mixin文件路径'
    export default {
        mixins:[ebookMixin],
        mounted(){
            this.setShowMenu('新的值')/* 如果不考虑先后顺序就，直接这样就可以了 */
            this.setFileName('新的值').then(()=>{/* 当需要在跟新vuex后调用最新值，这种是为了确保执行顺序，确保值是最新的 */
                console.log(this.filename)
            })
        }
    }
   ```

9. **vue国际化**
    ```js
    /* 新建一个配置国际化的页面 */
    import Vue from "vue"
    import VueI18N from "vue-i18n"
    import cn from "../lang/cn"
    import en from "../lang/en"
    Vue.use(VueI18N)
    const messages = {
    cn,
    en
    }
    const i18n = new VueI18N({
    locale: 'en', // 语言标识
    //this.$i18n.locale // 通过切换locale的值来实现语言切换
    messages
    })
    export default i18n;
    ```
    ```js
    /* main.js中引入国际化的页面 */
    import Vue from 'vue'
    import App from './App.vue'
    import i18n from './lang'
    new Vue({
    i18n,
    render: h => h(App)
    }).$mount('#app')

    ```
    ```html
    <!-- 在vue页面中直接开始使用就行了，格式如下 -->
    <span class="choose">{{$t('book.selectFont')}}</span>
    ```

10. **环境变量的配置**
    在项目根目录下创建一个`.env.development`的文件
    ```js
    <!-- .env.development页面 -->
    VUE_APP_RES_URL=http://192.168.50.11:81
    ```
    ```js
    <!-- 在需要引用全局变量的地方使用，使用格式如下： -->
    `${process.env.VUE_APP_RES_URL}/fonts/daysOne.css`
    ```

11. **切换主题**
    首先准备一个主题文件(就是样式颜色的文件)
    ```js
    /* 追加样式表的方法 */
    export function addClass(href){
        let link=document.createElement('link')
        link.setAttribute('rel','stylesheet')
        link.setAttribute('type','text/css')
        link.setAttribute('href',href)
        document.getElementByTagName('head')[0].appendChild(link)
    }
    /* 删除之前追加的样式表的方法 */
    export function removeClass(href){
        let links=document.getElementByTagName('link')/* 获取文档的所有link标签 */
        for(const i=links.length;i>=0;i--){/* 遍历link标签 */
            const link=links[i];
            if(link && link.getAttribute('href') && link.getAttribute('href')===href){/* 找到与参数一致的路径 */
                link.parentNode.removeChild(link)/* 然后删除它 */
            }
        }
    }
    /* 执行要删除的路径，写在这里方便管理 */
    export function removeAllClass(){
        removeClass('css路径1')
        removeClass('css路径2')
        removeClass('css路径3')
        removeClass('css路径4')
        removeClass('css路径5')
    }

    /* 切换主题 */
    changeThemes(id){
        removeAllClass();/* 在追加样式之前清空所有相关的样式表 */
        case 0:
            addClass('css路径1')
            break;
        case 1:
            addClass('css路径2')
            break;
        ···
    }
    ```

<<<<<<< HEAD
12. **修饰符**
    >当添加`.lazy`之前,input框绑定的`msg`是双向绑定的;当加了之后就不在实时的双向绑定，需要等到输入框失去焦点后或者按了回车键后才会更新视图绑定的数据
    <input v-model.lazy="msg" >

    >如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符
     如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符
13. **边角料**
    >1.因为你每用一次组件，就会有一个它的新实例被创建。

    >2.你可以使用不带参数的 v-bind (取代 v-bind:prop-name)。例如，对于一个给定的对象 post
    post: {
        id: 1,
        title: 'My Journey with Vue'
    }
    <blog-post v-bind="post"></blog-post>
    等价于
    <blog-post
    v-bind:id="post.id"
    v-bind:title="post.title">
    </blog-post>

    >3.`vm.$attrs`用于获取父作用域标签中除class和style之外的标签上的属性。
    `inheritAttrs: false`子组件默认会继承来自父组件的属性，这个就是禁止继承，这样一来子组件的根元素上面就不会有父组件传下来的属性啦
    ```js
    /*先申明子组件*/
        Vue.component("base-input", {
        inheritAttrs: false, //此处设置禁用继承特性
        props: ["label"],
        template: `
            <label>
            {{ label }}
            <input
                v-bind="$attrs"
            >
            </label>
        `,
        mounted: function() {
            console.log(this.$attrs);
        }
        });
    /*调用子组件*/
        <base-input
            label="姓名"
            class="username-input"
            placeholder="Enter your username"
            data-date-picker="activated"
            ></base-input>
    /*渲染结果*/
        运行后再chrome浏览器查看html结构时的结果如下：
        <label class="username-input">
              姓名
              <input placeholder="Enter your username" data-date-picker="activated">
        </label>
    /*不加禁用属性的情况下label标签是默认会继承来自父组件的属性，而input标签能继承则是因为我们使用了v-bind="$attrs"。*/
        <label class="username-input"  placeholder="Enter your username" data-date-picker="activated">
          姓名
          <input placeholder="Enter your username" data-date-picker="activated">
        </label>

    ```

    >4.深度作用域、事件穿透

    >5.对于绝大多数特性来说，从外部提供给组件的值会替换掉组件内部设置好的值。所以如果传入 type="text" 就会替换掉 type="date" 并把它破坏！庆幸的是，class 和 style 特性会稍微智能一些，即两边的值会被合并起来

    >6.我们推荐你始终使用 kebab-case 的事件名

    >7.一个组件上的 v-model 默认会利用名为 value 的 prop 和名为 input 的事件，但是像单选框、复选框等类型的输入控件可能会将 value 特性用于不同的目的。model 选项可以用来避免这样的冲突
    这里的 lovingVue 的值将会传入这个名为 checked 的 prop。同时当 <base-checkbox> 触发一个 change 事件并附带一个新的值的时候，这个 lovingVue 的属性将会被更新。
    `注意你仍然需要在组件的 props 选项里声明 checked 这个 prop。`
    ```js
        Vue.component('base-checkbox', {
        model: {
            prop: 'checked',
            event: 'change'
        },
        props: {
            checked: Boolean
        },
        template: `
            <input
            type="checkbox"
            v-bind:checked="checked"
            v-on:change="$emit('change', $event.target.checked)"
            >
        `
        })
     //使用该组件
        <base-checkbox v-model="lovingVue"></base-checkbox>
    ```
    >8.父子组件传值
    provider/inject：简单的来说就是在父组件中通过provider来提供变量，然后在子组件中通过inject来注入变量。

    需要注意的是这里不论子组件有多深，只要调用了inject那么就可以注入provider中的数据。而不是局限于只能从当前父组件的prop属性来获取数据。
    ```js
        //在父组件中
         provide: {
            for: "demo"
         }, 
         //当前父组件下面的任一子组件
         inject: ['for'],
=======
12. **真机联调**
    1. 通过ip地址在手机上面访问：在packge.json 的dev后面添加一个`--host 0.0.0.0`
    2. 在低版本手机上面测试可能出现白屏：
       >  `npm install babel-polyfill`,
            然后在main.js中`import "babel-polyfill"`
    3. 上滑组件的时候顶部出现白版： 在该组件中的`touchstart`加一个事件`touchstart.prevent`就可以了
    4. 项目部署：打包后把dist里面的目录放在后台的服务器根目录上面就可以了，如果想要更改目录，就得在`config/index.js`的          `assetsPublicPath:'/新目录'`

13. **动态组件引入**
    ```html
    <component :is="currenTab===1?EbookNavContent:EbookNavBook"></component>
    ```
    ```js
    import EbookNavContent from "../ebook/ebookNavContent";
    import EbookNavBook from "../ebook/ebookNavBook";
    data() {
        return {
        currenTab:1,
        EbookNavBook:EbookNavBook,
        EbookNavContent:EbookNavContent
        };
    },
>>>>>>> a90c8163dcb709798289cad873e9c9e31b8902ee
    ```
