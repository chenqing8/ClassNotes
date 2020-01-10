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