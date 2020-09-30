# webpack

## 1. webpack安装

注意: 先安装nodejs的最新版

   **全局安装webpack**
   ```
    npm i webpack webpack-cli -g
   ```
   
   **项目中安装webpack(推荐)**
   ```
    npm i webpack webpack-cli -D
   ```
   
## 2. webpack使用
   
### 2.1 webpack-cli
   
npm 5.2以上的版本提供了一个npx命令

npx 想要解决的主要问题, 就是调用项目内部安装的模块, 原理就是在node_modules下的.bin目录中找到对应的命令执行
   ```
   // 使用webpack 命令
   npx webpack
   ```
webpack4.0之后 可以实现0配置打包, 0配置的特点就是限制较多 , 无法自定义很多内容 

开发中常用的还是使用webpack配置进行打包构建

### 2.2 webpack配置

webpack有四大核心概念: 

 + 入口(enter): 程序的入口js
 
 + 输出(output): 打包后存放的位置
 
 + loader: 用于对模块的源代码进行转换
 
 + 插件(plugins): 插件目的在于解决loader无法实现的其他事情
 
 webpack.config.js
 ```
 const path = require('path')

module.exports = {
    entry: './src/index.js',
    output: {
        //  path.resolve() 解析当前相对路径的绝对路径
        // path: path.resolve('./dist/'),


        // path.join 传入两个参数 , (_dirname 当前根目录 , 相对路径) ,最后拼接成绝对路径
        path: path.join(__dirname, './dist/'),

        // 文件输出的名字
        filename: 'wanghan.js'
    },
    mode: 'production'  // 默认是production , 可以手动设置为development , 区别就是是否可以进行压缩混淆
}

 ```
 
1. npx webpack打包 , 默认选的配置文件webpack.config.js

   我们也可以自定义打包配置文件件,  如要换选用其他的配置文件 , npx webpack --config webpack.new.config.js
   
2. 但是每次如果切换打包配置文件 , 命令文件过于长了点 , 我们可以在package.json文件中配置命令

   ```
   "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --config webpack.new.config.js"
   },
   ```
   
   然后我们执行 **npm run build** 即可





### 2.3 开发时自动编译工具

每次要编译代码时, 手动运行 *npm run build* 就会变得很麻烦

webpack 中有几个不同的选项, 可以帮助我们的代码发生变化后自动编译代码

1. webpack's Wacth Mode  监视模式
2. webpack-dev-server  推荐~~
3. webpack-dev-middleware  中间件

多数场景中 , 可能需要使用的 *webpack-dev-server* , 但是不同的方式都可以了解一下 


#### 2.3.1 wacth

  **webpack is watching the files…**
  
  在webpack指令后加上 *--watch* 参数即可
  
  主要作用就是监视本地项目文件的变化 , 发现有修改的代码会自动编译打包 ,生成输出文件
   
   1. 配置package.json 的scripts 中 "watch" : "wabpack --watch"
   
   2. 运行 npm run watch
   
  以上是cli的方式设置watch的参数

  还可以通过配置文件对watch的参数进行修改

  ```
  const path = require('path')

  module.exports = {
      
      watch: true
  }

  ```
  
  
  #### 2.3.2 webpack-dev-server
  
   1. 安装 *devServer* :
       
       devServer需要依赖webpack , 必须在项目中依赖安装webpack
       
       
       ```
       npm i webpack-dev-server webpack -D
       ```
       
       
   2. index.html中修改  <script src="/wanghan/js"></script>

   3. 运行 **npm webpack-dev-server**

   4. 运行 **npx webpack-dev-server --hot --open --port 8090**
    
   5. 配置package.json的scripts: **"dev": "webpack-dev-server --hot --open --port 8090**
    
       + --contentBase
       
       *本地服务器默认打开根目录下的index.html文件 , 但是如果想要打开 src/index.html 或其他页面*
       
       *webpack-dev-server --contentBase ./src*
       
       + --open 自动开发
       
       + --port 配置端口
       
       + --hot 热模块替换
       
           不需要重新打包我们的文件 , 只需要像类似补丁的形式去更新我们变动的模块(打补丁)
           
       + --compress  压缩
       
    
   6. 运行 **npm run dev**
    
      devServer会在内存中生成一个打包好的wanghan.js文件, 专供开发时使用 , 打包效率非常高 , 修改代码后会自动打包以及刷新浏览器 , 用户体验非常好   
    
      以上是cli的方式设置devServer的参数
      
    
   7. 还可以通过配置文件对devServer的参数进行修改
   
   ```
   const path = require('path')

    module.exports = {
        devServer: {
            open: true,
            hot: true,
            compress: true,
            port: 8090,
            contentBase: './src'
        }
    }

   ```
   
   
   #### 2.3.3 html插件

 1. 安装html-webpack-plugin插件 *npm i html-webpack-plugin -D*
   
 2. 在 *webpack.config.js* 中的plugin节点下配置
   
   
    ```
    const HtmlWabpackPlugin = require('html-webpack-plugin')
    
    plugins: [
        new HtmlWabpackPlugin({
            filename: 'index.html',
            tamplate: './src/index.html'
        })
    ]

    ```
    
    **作用:**
    
      + devServer时, 根据模板在项目根目录下生成html文件 , 类似于devServer生成内存中的wanghan.js
      
      + devServer时, 自动打包引入 wanghan.js
      
      + build时, 打包时自动生成index.html
      
      
 #### 2.3.4 webpack-dev-middleware
 
 *webpack-dev-middleware* 是一个容器(wrapper) , 它可以把webpack处理后的文件传递给一个服务器(server).
 
 *webpack-dev-server* 在内部使用了它, 同时 , 它也可以作为一个单独的包来使用 , 以便进行更多的自定义的设置来实现更多的需求.
 
  1. 安装 express 和 webpack-dev-middleware: 
  
    ```
    npm i express webpack-dev-middleware -D
    ```
  
  2. 新建server.js
  
  ```
  const  express = require('express')

  const webpack = require('webpack')

  const webpackDevMiddleWare = require('webpack-dev-middleware')

  const config = require('./webpack.config.js')

  const app = express()

  const compiler = webpack(config)

  app.use(webpackDevMiddleWare(compiler, {
      publicPath: '/'
  }))

  app.listen(3000, function() {
      console.log('http://localhost:3000')
  })
  ```
  
  3. 配置package.json中的scripts : *"server" : "node server.js"*
  
  4. 运行 *npm run server*
  
  *注意: 如果要使用middleware, 必要要使用 html-webpack-plugin 插件, 否则html文件无法正常的输出到express服务器的根目录*
  
  接下来 我们要对webpack的配置文件做一些调整 , 以确保中间件(middleware)功能能够正确启用
  














