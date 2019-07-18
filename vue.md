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
    ```


5. **vuex**
  系统学习后
  可以看看vue微信读书3.4-4.2.mp4（有点绕，没大懂）

  import store from './store/index'必须小写

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
        }
    }
    /* 在需要使用vuex中的地方 */
    import {ebookMixin} from 'mixin文件路径'
    export default {
        mixins:[ebookMixin],
        mounted(){

        }
    }
   ```

