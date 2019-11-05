1. **什么是 nodejs**:基于 v8 引擎，渲染 js 的环境或工具
   + 安装 node
   + 把 js 放在 node 环境中执行
   + node 的安装可以从官网中下载安装，可以 nvm 安装，安装 node 后自动就安装了 npm(node 模块管理器,一个 js 的模块管理工具，基于 npm 可以下载安装 js 模块)
2. **如何在 node 中执解析渲染 s**
   + REPL 模式(输入-求值-输出-循环)
   + 直接基于 node 来执行 js 文件
      1. 在 DOS 窗口&终端窗口中执行(node XXX.js)
      2. 输入 node 命令就相当于 Chrome 的控制台
3. **node 的特点**这三个实际是一个特点、离开谁都是玩不转的
   + 单线程的：好处就是减少内存开销、操作系统的内存换页（如果某一个事情进入了但是被IO阻塞了，所以这个线程就被阻塞了，所以就有了非阻塞Io）
   + 基于 v8 引擎渲染：快
   + 异步无堵塞的 I/O 操作：不会傻等IO语句结束，而会执行后面的语句（）
   + 事件机制、事件环：不管是新用户的请求，还是老用户的IO完成，都将以事件方式加入事件，等调度。

   nodejs的所有IO都是异步的、回调函数调回调函数
   >`什么时候适合用nodejs`：
   善于IO不善于计算，因为nodejs最擅长任务调度，如果你的业务有很多的cpu计算，实际上也相当于这个计算阻塞了这个单线程，就不适合nodejs。
   当应用程序需要处理大量并发的IO的时候，而在客户端发出响应之前，应用程序内部并不需要进行非常复杂的处理时候，nodejs非常合适。nodejs也非常适合与webscoket配合。
4. **node 的基本使用**
   1. npm 的应用 1.目前工程化自动化开发（不一定是后台），都是基于 NODE 环境，基于 npm 管理模块，基于 webpack 实现模块之间的依赖打包，部署上线等。
   ```
     npm install xxx   把模块安装在当前目录下
     npm install xxx -g  把模块安装在全局目录下
     npm install cnpm -g
     cnpm install zepto
     ----
     npm install nrm -g 安装nrm切源工具，基于nrm把源切换到淘宝镜像上面
     nrm ls  查看当前可用源
     nrm use xxx 使用某个源
     这样处理完后，后期模块的管理依然基于npm即可
   ```
   2. 基于 yarn 安装;速度是比 npm 快很多的
   ```
   npm install yarn -g
   yarn add xxx
   yarn remove xxx
   使用yarn只能把模块安装到当前目录下，不能安装到全局环境下
   ```
5. **npm 配置清单和跑环境**
   在本地项目中基于 npm 或者 yarn 安装第三方模块
   ```
   第一步：在本地项目中，创建一个package.json的文件
   作用：把当前项目所有依赖的第三方模块信息(包含：模块名字以及版本信息)都能记录下来，可以在这里配置一些可执行的命令脚本等
   npm init -y 或yarn init -y
   第二步：安装
   第三步：部署的时候跑环境
   ```
6. **node安装之配置可执行命令脚本**
   安装全局的特点：所有的项目都可以使用这个模块
            缺点：容易导致版本冲突、安装在全局的模块不能基于CommonJs模块规范调取使用（简单来说就是不能再js中通过require来使用）
   安装在本地的特点：只能在当前项目中使用这个模块
            缺点：不能直接使用命令操作（安装在全局可以使用命令）
   `在npm的文件夹下可以自定义命令，参照xxx.cmd文件,只要有这个文件那个xxx就是一个可执行的命令，相当于npm，例如：yarn_text.cmd,,yarn_text add less`
   能否即安装在本地，也可以使用命令操作

7. **js运行环境**
    限制语言的能力的不是本身,而是语言的环境
    + 浏览器
      + 基本语法
      + dom
      + bom
      + ajax
      + 系统文件数据库
    + 服务器
      + 基本语法
      + 系统文件数据库
      + 可以操作本地文件
8. **nvm工具**
    node版本管理工具
9. **镜像**
    + 全局使用：就相当于把npm指向改变了变成国内镜像
    ```js
    $ npm config set registry https://registry.npm.taobao.org
    $ npm config get registry(或npm info express)
    ```
    + cnpm:这个就相当于重新下载了一个cnpm包
    ```js
    $ npm install -g cnpm --registry=https://registry.npm.taobao.org
    $ cnpm install express
    ```
10. **node的运行环境**
      直接在命令行中输入代码的就是REPL环境（一般不会用）
      新建一个js文件然后在node环境下运行（常用）
11. **模块化**
   + 内置模块
   + 第三方模块
   + 自定义模块
     - 创建一个模块(一个js文件一个模块)
     - 导出一个模块(module.export=name)
     - 引用一个模块并且调用

12. **打印当前目录的目录树**
    1. 实现效果
    2. 分析功能点
       + 当前目录结构
       + 分辨是文件还是文件夹

13. **内置模块 url**
      URL 统一资源定位符
      http://baidu.com/hot/user?id=1&pas=123#nihao
      [协议:]//[主机]/[地址]#[hash]
   ```js
    /**
    * url 类比json记忆
    * url.parse将url字符串转成对象
    * url.format将对象转成字符串
    */
   ```
14. **邮箱验证案例**
    + 如何通过node往邮箱中发送验证码，可以使用第三方模块(nodemailer)
    [nodemailer官网文档](https://nodemailer.com/about/)
      ```js
        'use strict';
        //引入第三方模块
        const nodemailer = require('nodemailer');
            //创建发送邮件对象
            let transporter = nodemailer.createTransport({
                host: 'smtp.qq.com',//通过lib/well-know/service.json,找到发送方邮箱
                port: 587,//端口号
                secure: false, // true for 465, false for other ports
                auth: {
                    user: '1150229571@qq.com', // 发送方邮箱地址
                    pass: 'kxlfqavqcooabgfd' // generated ethereal password
                }
            });

            // send mail
            transporter.sendMail({
                from: '"Fred Foo 👻" <1150229571@qq.com>', // sender address
                to: '1150229571@qq.com', // list of receivers
                subject: 'Hello ✔', // Subject line
                text: 'Hello world?', // plain text body
                html: '<b>Hello world?</b>' // html body
            },(err,data)=>{
              console.log(err)
            });
      ```
    + npm

15. **爬虫案例**
    + 获取目标网站(http.get)
    + 分析网站信息(cheerio插件   可以使用jq选择器 就不需要正则来取数据了)
    + 获取有效信息 下载或者其他操作
    
16.  **文件操作**
     
     
     
      + 读取文件
        - fs.readFile(file[,options],callback)
          fs.readFile(路径[,可选参数],回调)
        ```js
            const fs=require('fs')
            const path=require('path')
            //此处的`./`相对路径，是相对于执行node命令的路径   
            
            //我们自己拼接路径的时候徐亚考虑不同操作平台上的斜杠方向问题
            //let filename=__dirname+'\\'+'hello.txt';
            let filename = path.join(__dirname,'hello.txt')
        	fs.readFIle(filename,(err,data)=>{//error-first概念就是回调的第一个参数总是err
             //data的数据类型是Buffer对象，里面保存的是一个一个的字节(字节数组)
             //把buffer转化为字符串，可以调用toString()方法，不传uft8参数，默认也是utf8
           })
        ```
        > 解决在文件读取中 ./ 相对路径问题
        > 解决办法：\_\__dirname \\  \_\_filename
        >
        > \_\_dirname:表示当前正在执行的js的文件目录、其实是本地的
        > \_\_filename:表示当前正在执行的js的完整路径、其实是本地的

17.  **服务器**

```js
/**
 * 1.创建一个服务,并且启动
 */
var http = require('http');
var server = http.createServe();//创建一个http服务对象
server.on('request',(req,res)=>{//监听用户的请求事件(request事件)
    //body……
    //解决乱码思路：服务器通过设置http响应报文头，告诉浏览器使用相应的编码来解析网页,如果想要解析HTML文档，就用text/html
    res.setHeader('Content-Type','text/plain;chartset=utf-8')
    res.write('Hello World!!!你好世界')//就会显示在页面上
    res.end()//必须要res.end()来结束服务器请求
})
server.listen(8080,()=>{
    console.log('服务器启动了')
})
//通过访问localhost:8080


  /*
    2.根据用户的不同请求服务器做出不同的响应
  */
http.createServer((req,res)=>{
  // 获取用户的请求路径
  res.setHeader('Content-Type','text/html;charset=utf-8')
  if(req.url=='/'){
    res.end('欢迎进入')
  }else if(req.url=='/login'){
    res.end('这是登录页')
  }else{
    res.end('这是首页')
  }
}).listen(8081,()=>{
  console.log('http://localhost:8081');
})

/**
 * 3.根据用户的不同请求服务器做出不同的响应(响应现有html文件、html中包含了image)
 */
const fs= require('fs')
const path=require('path')
http.createServer((req,res)=>{
  // 获取用户的请求路径
  if(req.url=='/'){
    fs.readFile(path.join(__dirname,'welcome.html'),(err,data)=>{
      if(err){
        return false;
      }
      res.end(data)
    })
  }else if(req.url=='/images/index.png'){//请求一张图片的时候
    fs.readFile(path.join(__dirname,'images','index.png'),(err,data)=>{
      if(err){
        return false;
      }
      res.setHeader('Content-Type','image/png')
      res.end(data)
    })
  }else if(req.url=='/css/index.css'){//请求css文件的时候
    fs.readFile(path.join(__dirname,'css','index.css'),(err,data)=>{
      if(err){
        return false;
      }
      res.setHeader('Content-Type','text/css')
      res.end(data)
    })
  }else{
    fs.readFile(path.join(__dirname,'index.html'),(err,data)=>{
      if(err){
        return false;
      }
      res.end(data)
    })
  }
}).listen(8082,()=>{
  console.log('http://localhost:8082');
})
/*
* 4. 动态加载所有的资源请求，创建Apache服务器
*/
const http= require('http')
const fs= require('fs')
const path=require('path')
const mime=require('mime')//解析资源文件的contentType类型
http.createServer((req,res)=>{
  // 得到公共的路径
  let publicPath = path.join(__dirname,'public');
  // 用公共路径拼接资源请求的路径，就是本地文件的路径  
  let filename = path.join(publicPath,req.url);
  fs.readFile(filename,(err,data)=>{
      if(err){
          return false;
      }
      res.setHeader('Content-Type',mime.getType(filename))//这里就可以动态的生成资源文件的类型
      res.end(data)
  })
}).listen(8082,()=>{
  console.log('http://localhost:8082');
})
```

> 1.request:包含了用户请求报文中的所有内容，通过request对象可以获取所有用户提交过来的数据
>
>    response：用来向用户响应一些数据，当服务器要向客户端响应数据的时候必须使用response对象
>
> 2.有了request对象和response对象，就即可以获取用户提交的数据，也可以向用户响应数据了
>
> 3.请求的url地址，就是一个标志符，是用来做判断的，具体跑到那个页面是通过服务器来处理
>
> 4.你可以给img的src地址写一个xxx.css,此时只需要到时候发起图片地址xxx.css地址请求的时候，服务器返回一张图片就可以了

18. **Try-Catch**

> 异步操作，try-catch无法捕获异常
>
> 对于异步操作，要通过判断错误号(err.code)来进行出错处理

















































