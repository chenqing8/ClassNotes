# webpack项目：
1.提供了约定大于配置，默认约定了打包的入口是`src->index.js`,打包后输出`dist->main.js`
2.4.x中新增了`mode`选项，决定是否压缩代码，这是必须选项
3.`webpack-dev-server`实时打包好的`mian.js`是托管到了内存中，所以在项目目录中看不到，但是我们假装还是在项目根目录中是有的
4.带`s`的都是数组，不带`s`的是对象

# webpack安装
1. 本地安装webpack，命令：`npm install webpack webpack-cli -D`
2. webpack可以进行0配置

## 样式处理：
1.  **css-loader**：用来接续`@import`这种语法，把代码拼接成一个文件
    **style-loader**：用来把拼接好的代码文件插入在html文件中
2.  **loader**的特点是希望单一
        loader的用法：字符串表示只用一个loader；数组表示可以用多个loader
        loader的顺序，默认是从右向左执行；从下到上执行，例如`['style-loader','css-loader']`表示先把css代码拼接后在插入到html中
        loader还可以写成一个对象的形式`['style-loader',{loader:'css-loader'}]`,重要的是可以传参数`['style-loader',{loader:'css-loader'，options：{}}]`
3.  **修改优先级**默认不给参数的情况下，css代码是插入在html的head标签底部的，这样自己的样式优先级就没有那么高了，，，解决办法:
       ```javascript
        [{
            loader:'style-loader',
            options：{
            insertAt:'top'
            }
        }，
        'css-loader']
       ```
4.  **使用less文件的话**：
       ```javascript
        [{
            loader:'style-loader',
            options：{
               insertAt:'top'
            }
        }，
        'css-loader',
        'less-loader']
        ```
5.  **抽离css样式的插件**：`mini-css-extract-plugin`
    使用：new 插件，然后替换
       ```javascript
        [
        MiniCssExtractPlugin.loader，
        'css-loader',
        'less-loader']
        ```
6.  **自动添加前缀**：使用`postcss-loader、autoprefixer`,例如：
       ```javascript
        [
        MiniCssExtractPlugin.loader，
        'css-loader',
        'postcss-loader',
        'less-loader'
        ]
      ```
       然后需要新建一个`postcss.config.js`文件，内容如下：
       ```javascript
       module.exports={plugins:[require('autoprefixer')]}
       ```
7.  **css压缩**在有`mini-css-extract-plugin`的前提下，还需要添加`optimize-css-assets-webpack-plugin`和js压缩插件；例如：
       ```javascript
     optimization: {
       minimizer: [new UglifyJsPlugin({}), new OptimizeCSSAssetsPlugin({})],
     }
     ```
## js处理：
1.  **将es6装换为es5**：
     ```
     npm install babel-loader@8.0.0-beta.0 @babel/core @babel/preset-env webpack 
     ```
     ```javascript
     {
      test: /\.js$/,
      use: {
        loader: 'babel-loader',
        <!-- loader:可以是自定义的js文件，，，loader：path.resolve(__dirname,'js的所在的文件夹（与配置文件同级的）','js文件名'） -->
        options: {
          presets: ['@babel/preset-env'],//把es6转换为es5
          plugins: [require('@babel/plugin-proposal-object-rest-spread')]
        }
      }
    }    
    ```

2.  **处理js语法以及校验**：
    {
        test：/\.js$/,
        use:{
            loader:eslint-loader',//相当于lessloader，需要安装eslint eslint-loader
            options:{
               enforce：'pre'//将lint放在其他js规则之前执行
            }
        }
    }
    然后在更目录下放入.eslintrc.json文件，内容可以在官网中演示=>底部展开进行配置，然后下载
3.  **全局变量引入**：
    1.  暴露全局$:可以用expose-loader
      11. import $ from 'expose-loader?$!jquery'//用？！的格式表示将jquery用$的形式展示
      22. 
          <!-- 需要$的地方 -->
          import $ from 'jquery'
          <!-- 配置文件 -->
          {
           test:require.resolve('jquery'),
           use:'expose-loader？$'
          }
    2.  在每个模块中注入$对象
        plugins: [
             ...
             new webpack.ProvidePlugin({
               $: 'jquery'
             }),
             ...
        ]
    3.   外部引入的库不需要重新进行打包：
         例如，你使用了cdn,可是你却import $ from 'jquery'//这就代表把jQuery打包为一个模块，这是我们不想要的
         externals: {
          jquery: "$"
         },

## 图片处理：
1.  **打包图片** ：
    11. ***在js中创建图片来打包***，需要用到file-loader
        let image= new Image();
        image.src='./logo.png';<!--此时只会当做一个字符串不会被打包，处理办法：require或者import logo from ‘./logo.png’然后image.src=logo，，，需要使用file-loader-->
        document.body.append(image);
    22.  file-loader的作用就是，默认会在内部生成一张图片发射在build目录下，然后把生成的图片的名字返回回来
    33.  {
            test:/\.(png|jpe?g|gif)$/,
            use:'file-loader'
         }
    44.  ***在html中引入图片***，需要用到html-withimg-loader,效果就是图片路径编程hash值
         {
                 test:/\.html$/,
                 use:'html-withimg-loader'
         }
    55.  ***url-loader***：一般情况下我们不会直接用file-loader，因为他是直接将图片用于http请求，，这样就浪费了资源，所以我们可以限制他小于多少大小的时候       就    用base64 ，大于多少才用file-loader
          {
                  test:/\.(png|jpe?g|gif)/,
                  use:{
                          loaders:'url-loader',
                          options:{
                                  limit:200*1024<!--当这个图片小于200K-->
                          }
                  }
          }
##文件分类：
1.  有options就使用outputpath：
        {
                        test:/\.(png|jpe?g|gif)/,
                        use:{
                                loaders:'url-loader',
                                options:{
                                        limit:200*1024，<!--当这个图片小于200K-->
                                        outputPath:'img/'
                                }
                        }
        }
2.  有插件就在filename中直接添加

## 多页打包：
1.  module.exports = {
        entry: {<!--可以是多个入口文件-->
          'home': './src/pages/home/main.js',
          'about':'./src/pages/about/main.js'
        },
        output: {<!--有几个入口文件就有多少出口文件，通过[name]来实现-->
          path: path.resolve(__dirname, './build'),
          filename: '[name].js'
        },
        plugins: [
             ...
            new HtmlWebpackPlugin({<!--如果想要产生多个html页面就写多个HtmlWebpackPlugin-->
                template: 'index.html',<!--想要引用html的模板文件-->
                title: 'home',
                filename: 'home.html',<!--生成后的文件名字-->
                chunck:['home']<!--这个文件放home入口代码块，可以放多个-->
            }),
            new HtmlWebpackPlugin({
                template: 'index.html',
                title: 'about',
                filename: 'about.html',
                chunck:['about','home']
            })
             ...
        ]
   }

## 配置source-map
1. 当是生产模式的时候，如果项目出错，你想找问题的时候就只能看到压缩后的代码，解决办法就是进行源码映射
2. **代码调试**
   module.exports = {
        entry: {         ...        },
        output: {         ...        },
        plugins: [             ...        ],
        // devtool:'source-map',<!--增加映射文件可以帮我们调试代码，最暴力的就是直接写source-map，会生成一个source-map的文件，出错了会标识在哪一行那一列-->
        // devtool：'eval-source-map',<!--不会产生单独的文件，但是可以显示行和列-->
        // devtool：'cheap-module-source-map',<!--会产生单独的文件，但是不可以显示列-->
         devtool：'cheap-module-eval-source-map',<!--不会产生单独的文件，集成在打包的文件中 ，不可以显示列-->
   }

## watch的用法
1.**监控、自动打包部署** 
module.exports = {
        entry: {         ...        },
        output: {         ...        },
        plugins: [             ...        ],
        watch:true,<!--监控代码是否有变化，有变化就会实时打包，，前提是你在终端输入了打包的命令-->
        watchOptions:{
                poll:1000,<!--每秒问我1000次-->
                aggregateTimeout:500,<!--防抖，，，停止后500毫秒内我输入的东西只打包一次-->
                ignored:/node-modules/,<!--可以忽略调的监控文件-->
        }<!--监控的属性设置-->
        // devtool:'source-map',<!--增加映射文件可以帮我们调试代码，最暴力的就是直接写source-map，会生成一个source-map的文件，出错了会标识在哪一行那一列-->
        // devtool：'eval-source-map',<!--不会产生单独的文件，但是可以显示行和列-->
        // devtool：'cheap-module-source-map',<!--会产生单独的文件，但是不可以显示列-->
        devtool：'cheap-module-eval-source-map',<!--不会产生单独的文件，集成在打包的文件中 ，不可以显示列-->
   }

## webpack小插件：
1. **copy-webpack-plugin**，将from目录拷贝到to目录中
   new CopyWebpackPlugin([
      { from: '**/*', to: 'relative/path/to/dest/' },
   ]),
2. **clean-webpack-plugin**，每次打包前都清空一下当前目录
   new CleanWebpackPlugin('**/*'),
3. **bannerPlugin**,版权声明插件，内置的
   new webpack.BannerPlugin('makeBychenqing');<!--会被添加在每个打包的js文件头部-->

## webpack跨域
1. 代理：
   devServer:{
           proxy:{
                   '/api':{<!--通过重写的方式吧请求代理到express服务器上面-->
                           target:'http://localhost:3000',
                           pathRewrite:{
                                   '/api':''
                           }
                   }
           }
   }

2. 前端只是想单纯的来模拟数据：
   before(app){
           app.get('user',(req,res)=>{
                   res.json({name : 'cq'})
           })
   }
3. 有服务端了就不需要代理，webpack端口用服务端的端口

## resolve解析查找第三方包：
1.  **别名、查找顺序**
    <!--main.js-->
    import 'bootstrp';<!-- 用于在js文件中引入一个库文件，他首先是去库的package.json的main的路径下找，你希望改变他的路径，有三种方式，1.别名2.写死3.mainfields改变库文件查找顺序 -->
   11. <!-- config.js -->
   resolve:{
     modules:['./src/components','node_modules']，<!--只在当前两个目录下面查找第三方包文件-->
     alias:{<!--别名-->
      bootstrp: 'bootstrp/dist/css/index.css'
     }
   }
   22. <!-- main.js -->
   import 'bootstrp/dist/css/index.css';
   33. <!-- config.js -->
   resolve:{
     modules:['./src/components','node_modules']，<!--只在当前两个目录下面查找第三方包文件-->
     mainfields:['style','main']<!-- 改变在库文件中查找文件的顺序 -->
   }
2.  **自动添加后缀**
   <!-- 当前引入，你没有加后缀，怎么让程序自己识别呢 -->
   <!-- main.js -->
   import nav from './nav'
   <!-- cinfig.js -->
    resolve:{
     extensions:['.js','.css','.vue','.json']//在没有后缀的引入文件，，自动根据这个数组，添加后缀，来查找是否有该文件，有就停止向后继续查找
     
   }

## 定义环境变量：DefinePlugin、webpack-merge
1. 开发的时候和上线时候的环境是不一样的
2. 
<!-- main.js -->
        let url=''
        if(DEV){
                url="http://192.168.10.10:3000"
        }else{
                url="http://222.168.10.10:8000"
        }
<!-- config.js -->
        new webpack.DefinePlugin({
          'DEV': "'dev'",<!-- dev必须后面添加个双引号，或者json.stringify('dev'),通过改变dev的值来判断是生产模式还是开发模式‘production’ -->
        }),
3. 使用wenpack-merge来判断当前环境

## webpack打包优化点：
1. ***noParse***
   去解析指定库文件的依赖关系：
        <!-- main.js -->
        import jq from 'jquery'
        <!-- config.js -->
        module.exports = {
                mode:'development',
                entry:'./src/index.js',
                output:{...},
                module:{
                        noParse:/jquery/,//不去解析jquery中的依赖库
                        rules:[...]
                },
                plugins:[...]
        }

2. ***IgnorePlugin不能因为只用一个文件却要打包100个多余的文件、exclude不能因为查找一个文件却要查找100个多余的文件***
   ****exclude**不能因为查找一个文件却要查找100个多余的文件**
        在匹配/\.js$/的时候，默认情况下他也会去nodule-mode中去找，为了解决这个问题，可以用exclude来排除node-module或者include表示只在那个目录下面查找，二选一就行了。
        module.exports = {
                ...
                module: {
                loaders:[
                {
                        test: /\.js[x]?$/,
                        exclude: /node_modules/,<!-- 在查找js文件的时候排除查找node_modules中的js文件 -->
                        <!-- include：path.resolve('src') 在查找js文件的时候只查找src中的js文件-->
                        loader: 'babel-loader?presets[]=es2015&presets[]=react',
                },
                ]
                }
        };


    ****IgnorePlugin**不能因为只用一个文件却要打包100个多余的文件** 
        module.exports = {
                ...
                  new Webpack.IgnorePlugin(/\.\/locale/,/moment/),//moment这个库中，如果引用了./locale/目录的内容，就忽略掉，不会打包进去,但是如果想用个别的就单独import引入，不能因为只用一个文件却要打包100个多余的文件
                ...
        };
3. **动态链接库减少包的体积**
      DllPlugin、DllReferencePlugin
4. ***自动去除没有用的代码***
    比如通过**import**引用了export导出的sum和add两个方法，可是程序中却没有用到add方法，只用了sum方法，此时生产环境中打包后的文件中是没有add方法的。
    **tree-shaking**吧没用的代码自动删除掉，但是只支持import，不支持require
    **es6模块会把导出的结果放在default上面**
5. ***在webpack中会自动省略可以简化的代码***
    let a=1;
    let b=2;
    let c=3;
    let d=a+b+c;
    console.log(d);<!--此时，打包后的文件里面没有申明的ABCD变量，直接就是代码console.log(6)-->

## 多线程打包：
1. HappyPack它能让webpack同时处理多个任务，js可以css也可以这样操作
2. module.exports = {
        module: {
                rules: [
                {
                        test: /\.js$/,
                        // 将对.js文件的处理转交给id为babel的HappyPack的实列
                        use: ['happypack/loader?id=babel'],
                }
                ]
        }，
        plugins: [
                /****   使用HappyPack实例化    *****/
                new HappyPack({
                // 用唯一的标识符id来代表当前的HappyPack 处理一类特定的文件
                id: 'babel',
                // 如何处理.js文件，用法和Loader配置是一样的，就把rules的对loader的配置搬下来就行了
                loaders: ['babel-loader']
                })
        ]
   };

##代码抽离
 module.exports = {
        optimization: {
                splitChunks: {<!--抽离第三方包，只会在多个入口文件的时候生效，单页应用没用的-->
                        cacheGroups:{ // 缓存
                                common:{//公共部分
                                        chunks:'initial',
                                        minSize:0,//文件大于0的
                                        minChunks:2//被用了两次就提取出来
                                },
                                vendor: { // 公共第三方部分
                                        priority: 0, // 缓存组优先级，默认从上到下，会导致库文件也会被提取到公共部分去
                                        chunks: "initial", // 必须三选一： "initial" | "all" | "async"(默认就是async) 
                                        test: /react|lodash/, // 正则规则验证，如果符合就提取 chunk
                                        name: "vendor", // 要缓存的 分隔出来的 chunk 名称 
                                        minSize: 30000,
                                        minChunks: 1,
                                        enforce: true,
                                        maxAsyncRequests: 5, // 最大异步请求数， 默认1
                                        maxInitialRequests : 3, // 最大初始化请求书，默认1
                                        reuseExistingChunk: true // 可设置是否重用该chunk
                                }
                        }
                }
        },
 }

 ##代码懒加载需要安装@babel/plugin-syntax-dynamic-import
 <!-- js文件 -->
 click(){
         import ('./scource.js').then(res=>{<!--这个属于动态加载import，所以需要使用插件-->

         })
 }
 <!-- config.js -->
      {
      test: /\.js$/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env'],//把es6转换为es5
          plugins: [require('@babel/plugin-syntax-dynamic-import')]//用于使用动态的import
        }
      }
    }    

##热更新need plugin作为支持HotModuleReplacementPlugin内置插件

1.  **HotModuleReplacementPlugin**热更新插件
    **NameModulesPlugin**打印更新模块的名字
module.exports = {
    entry: {
     ...
    },
    devtool: 'inline-source-map',
    devServer：{
        hot :true,
        port :3000,
        open :true,
        contentBase:'./dist'
    }
    plugins: [
      ...
      new webpack.HotModuleReplacementPlugin()
      new webpack.NameModulesPlugin()
    ],
    output: {
     ...
    }
  };
 
## tapable

## loader
**loader可以自定义**：
1. loader：path.resolve(__dirname,'js的所在的文件夹（与配置文件同级的）','js文件名'）
2.  
       module.exports = {
        entry: {         ...        },
        output: {         ...        },
        resolveLoader:{
                alias:{
                        loader1:path.resolve(__dirname,'js的所在的文件夹（与配置文件同级的）','js文件名'）
                }
        },
        module:{
                rules:[
                        {
                                test:/\.js$/,
                                use:'loader1'
                        }
                ]
        }
      }
3. 
       module.exports = {
        entry: {         ...        },
        output: {         ...        },
        resolveLoader:{
                <!-- 告诉loader在node_modules中查找，找不到就在第二个地址中查找 -->
              modules:['node_modules',path.resolve(__dirname,'js的所在的文件夹（与配置文件同级的）')]
        },
        module:{
                rules:[
                        {
                                test:/\.js$/,
                                use:'loader1'
                        }
                ]
        }
      }

**loader配置**
1. loader的执行顺序默认就是从右向左，从下到上
2. loader一般分为pre在前面的loader，post在后面的loader,正常的normal
4. loader 顺序pre、normal、inline、post
3. module:{
                rules:[
                        {
                                test:/\.js$/,
                                use:'loader1'
                        }，
                        {
                                test:/\.js$/,
                                use:'loader2'
                        }，
                        {
                                test:/\.js$/,
                                use:'loader3'
                        }
                ]
   }
   执行结果：loader3，loader2，loader1，如果想要改变执行顺序可以通过enforce，
    module:{
                rules:[
                        {
                                test:/\.js$/,
                                use:'loader1'，
                                enforce：'pre'
                        }，
                        {
                                test:/\.js$/,
                                use:'loader2'
                        }，
                        {
                                test:/\.js$/,
                                use:'loader3'，
                                enforce：'post'
                        }
                ]
    }
5. 行内loader，就是嵌套在代码里面
   <!-- main.js -->
   let str=require('行内的loader文件!./a.js')<!-- !表示导出在前面的那个文件里面 -->
   let str=require('-！行内的loader文件!./a.js')<!-- -!表示pre和normal的loader不执行 -->
   let str=require('！行内的loader文件!./a.js')<!-- !表示normal的loader不执行 -->
   let str=require('！！行内的loader文件!./a.js')<!-- !表示什么都不执行，只执行inlineloader -->

##file-loader、url-loader
1. 目的就是将图片生成一个MD5，发射到dist目录下，然后返回一个图片路径