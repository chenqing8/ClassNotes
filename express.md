
## express
     通过express 框架 书写api
     什么是api呢？ajax请求数据，前后端分离  $.get('url',()=>{})

#### 安装
####  使用
    ```js
        const express= require('express')
        var bodyParser = require('body-parser')
        const app=express();
        app.use(bodyParser.urlencoded({ extended: false }))
        // 这个就相当于后端提供的api
        app.get('/user/login',(req,res)=>{
            let {us,ps}=req.query;//req.query是用来接受用户的参数，req是请求
            res.send({err:0,msg:'ok'})//res是响应
        })
        app.post('/reg',(req,res)=>{
            // 在post请求中如果你想解析req.body，需要使用第三方插件body-parse，否则下面的req.body会为undefined
            let {us,ps}=req.body;//req.body是用来接受用户的参数，req是请求
            res.send({err:0,msg:'ok'})//res是响应
        })
        // 端口3000的服务器
        app.listen(3000,()=>{
            console.log('serve start');
        })
        //首先node init.js启动服务器，然后可以通过http://loaclhost:3000/user/login来访问接口,也可以使用IP地址访问，只要在一个局域网内，其他电脑也可以访问
    ```
#### api接口构成
     IP
     port
     pathname 语义化
     method get post
     接受用户的参数
#### api接口的书写
    + 接受参数
      + get=>req.query
      + post=>req.body 需要用到body-parse插件来进行解析
        - 注意数据格式 ： json 、x-www-form-urencode 、formdata
