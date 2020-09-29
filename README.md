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
3. webpack-dev-middleware  中间键

多数场景中 , 可能需要使用的 *webpack-dev-server* , 但是不同的方式都可以了解一下 


#### 2.3.1 wacth
  
  在webpack指令后加上 *--watch* 参数即可
  
  主要作用就是监视本地项目文件的变化 , 发现有修改的代码会自动编译打包 ,生成输出文件
   
   1. 配置package.json 的scripts 中 "watch" : "wabpack --watch"
   
   2. 运行 npm run watch
   
  以上是cli的方式设置watch的参数

  还可以通过配置文件对watch的参数进行修改




















