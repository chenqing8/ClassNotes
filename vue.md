# vueCli3.0

1. **图形化界面**
>在任意位置打开图形化界面：vue ui
    前提是要npm install -g @vue/cli

>修改打包后路径问题，新建一个vue.config.js(这个名字是官方推荐的)
    ```js
        /* vue.config.js 这个名字是vuecli3.0默认的*/
        module.exports={
            baseUrl:process.env.NODE_ENV==='production'
            ?'./'
            :'/'
        }
    ```
>less的使用：
 npm install less less-loader -D
 然后在vue文件中直接使用即可<style lang="less" scoped>

>web字体引入
`方案一：`
    在index.html中，字体文件和index.html在同一个目录下
    <link rel="stylesheet" href="<%= BASE_URL %>web_font/daysOne.css">
    然后直接使用
    font-family: "Days One"
`方案二：`
    在main.js中引入css,然后直接使用即可
    import '../public/web_font/daysOne.css'

>