# webpack

### 1. webpack安装

注意: 先安装nodejs的最新版

   **全局安装webpack**
   ```
    npm i webpack webpack-cli -g
   ```
   
   **项目中安装webpack(推荐)**
   ```
    npm i webpack webpack-cli -D
   ```
   
### 2. webpack使用
   
#### 2.1 webpack-cli
   
npm 5.2以上的版本提供了一个npx命令

npx 想要解决的主要问题, 就是调用项目内部安装的模块, 原理就是在node_modules下的.bin目录中找到对应的命令执行
   ```
   // 使用webpack 命令
   npx webpack
   ```
webpack4.0之后 可以实现0配置打包, 0配置的特点就是限制较多 , 无法自定义很多内容 

开发中常用的还是使用webpack配置进行打包构建

#### 2.2 webpack配置

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























