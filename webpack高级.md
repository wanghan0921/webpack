## 1. HTML中img标签的图片资源处理

  1. 安装 npm install -S html-withimg-loader
  
  2. 在webpak.config.js文件中添加loader
  
    ```
    {
      test: /\.(html|html)$/i,
      loader: 'html-withimg-loader'
    }
    ```
   使用时, 只需要在html中正常引用图片即可, webpack会找到对应的资源进行打包, 并修改html中的引用路劲
   
## 2. SPA 单页面应用(Single Page Application)

## 3. 多页应用打包

    ```
    // 修改为多入口
    entry: {
        index: './src/index.js',
        other: './src/other.js'
    },
    output: {
        //  path.resolve() 解析当前相对路径的绝对路径
        // path: path.resolve('./dist/'),


        // path.join 传入两个参数 , (_dirname 当前根目录 , 相对路径) ,最后拼接成绝对路径
        path: path.join(__dirname, './dist/'),

        // 文件输出的名字
        // 多入口无法对应一个固定的出口 , 所以修改filename为 [name] 变量
        filename: '[name].js'
    },
    mode: 'development',  // 默认是production , 可以手动设置为development , 区别就是是否可以进行压缩混淆
    // watch: true  // 开启则会打开监视模式
    devServer: {
        open: true,
        hot: true,
        compress: true,
        port: 8090,
        // contentBase: './src'
    },
    plugins: [
        // 如果用了html插件 , 需要手动配置多入口对应的html1文件 将指定对应的输出文件
        new HtmlWabpackPlugin({
            filename: 'index.html',
            template: './src/index.html',
            // 从入口entry的名字来
            chunks: ['index']
        }),
        new HtmlWabpackPlugin({
            filename: 'other.html',
            template: './src/other.html',
            chunks: ['other']
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin({
            patterns:[
               { from: path.join(__dirname, 'assets'), to: 'assets' }
            ]
        }),
        new webpack.BannerPlugin('涂心仪')
    ],
    ```
    
    
 ## 3. 多页应用打包
 
  可以通过*expose-loader*进行全局变量注入, 
  
  同时也可以使用内置插件 *webpack.ProvidePlugin*对每个模块的闭包空间注入一个变量, 自动加载模块, 而不必到处 import 或 require
  
  + expose-loader将库引入到全局作用域
  
      1. 安装 *npm i expose-loader -D*
      
      2. 配置loader
      
      ```
      {
        // require.resolve找到jquery的绝对路径
        test: require.resolve('jquery'),
        use: {
          loader: 'expose-loader',
          options: '$'
        }
      }
      ```
      
  + webpack.ProvidePlugin 将库自动加载到每个模块
  
    1. 引入webpack
    
      ```
        const webpack = require('webpack')
      ```
      
    2. 创建插件对象
    
    要自动加载jquery , 我们可以将两个变量都指向对应的node模块
    
    ```
    new webpack.ProvidePlugin({
      $: 'jquery',
      jQuery: 'jquery'
    })
    ```
 
 
 ## 4. Development / Production 不同配置文件打包
 
 项目开始时一般需要使用两套或多套配置文件 , 用于开发阶段打包(不压缩代码, 不优化代码, 增加效率 ) 和 上线阶段打包 (压缩代码 , 优化代码 , 打包后直接上线使用)
 
 抽取三个文件 :
 
   + webpack.base.js
   
   + webpack.prod.js
   
   + webpack,dev.js
   
  步骤如下: 
  
   1. 将开发环境和生产环境公用的配置放入base中, 不同的配置各自放入prod或dev文件中
   
   2. 然后再dev和prod中使用webpack-merge把自己的配置与base的配置进行合并后导出
   
      *npm i webpack-merge -D*
   
   3. 将package.json中的脚本参数进行修改, 通过 *--config* 手动指定特定的配置文件
   
      ```
      const {merge} = require('webpack-merge')

      const baseConfig = require('./webpack.base.js')

      module.exports = merge(baseConfig, {
          mode: 'production',
          devtool: 'cheap-module-source-map'
      })

      ```
      
 ## 5. 定义环境变量
 
  除了区分不同的配置文件进行打包 , 还需要在开发时知道当前的环境是开发阶段或上线阶段, 所以可以借助webpack内置插件**DefinePlugin**来定义环境变量
  
  最终实现开发阶段和上线阶段的api的地址自动切换等目的
  
   1. 引入webpack
   
      ```
      const Webpack = require('webpack')
      ```
      
   2. 创建插件对象 ,并定义环境变量
   
      ```
      new Webpack.DefinePlugin({
        DEV: 'true'
      })
      ```
      
   3. 在src打包的代码环境下可以直接使用
   
   
  ## 6. 使用devServer解决跨域问题
  
  在开发阶段很多时间需要使用到跨域 
  
  **跨域** 受制于浏览器的同源策略 , 所谓同源是指 , 域名 , 端口 , 协议 相同
  
  开发阶段往往会遇到上面这种情况 , 也许将来上线后 , 前端项目会和后端项目部署在同一个服务器下 , 并不会有跨域问题 , 
  
  但由于开发时会用到 webpakc-dev-server, 所以一定会有跨域问题
  
  目前解决跨域主要的方案有 : 
  
     + jsonp (淘汰)
     
     + cors
     
     + http proxy
     
   此处介绍的使用devServer解决跨域, 其实原理就是 http proxy
   
   将所有的ajax请求发送给devServer服务器 , 再由devServer服务器做一次转发 , 发送给数据接口服务器
   
   由于ajax请求是发送给devServer服务器的 , 所以不存在跨域问题 , 而devServer由于是用node平台发送的http请求 ,
   
   自然也不涉及跨域问题 , 可以完美解决!
   
   [devServer-proxy](https://webpack.js.org/configuration/dev-server/#devserverproxy)
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
