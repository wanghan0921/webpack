
## 1. production模式打包自带优化

  + **tree shaking**
  
    tree shaking是一个术语, 通常用于打包时移除JavaScript中未引用的代码(dead-code), 它依赖于ES6模块系统中的import 和 export的静态结构特性.
    
    开发时引入一个模块, 如果只使用其中一个功能, 上线打包时只会把用到的功能打包进bundle, 其他没有用到的功能不会打包进来, 可以实现最基础的优化.
    
    *import* 静态导入, 只能在根节点写
    
    *require* 是动态导入 , 可以在if的大括号里面写
    
    
  + **scope hoisting**
  
    scope hoisting的作用是将模块之间的关系进行结果推测, 可以让Webpack打包出来的代码文件更小, 运行更快
    
    scope hoisting的实现原理其实也很简单: 分析出模块之间的依赖关系, 尽可能的把打散的模块合并到一个函数中去, 但前提是不能造成代码的冗余
    
    因此只有那些被引用了一次的模块才能被合并
    
    由于scope hoisting 需要分析出模块之间的依赖关系, 因此源码必须采用es6模块化语法, 不然它将无法生效
    
    原因和 tree shaking一样
    
    
  + **代码压缩**
  
    所有代码使用UglifylsPlugin插件进行压缩, 混淆
    
    
 ## 2. 将css提取到独立文件中
  
  > 原因: 多个模块使用是可以复用,不用每个模块都插入一个style标签
 
  *mini-css-extract-plugin*是用于将css提取为独立的文件的插件, 对每个包含css的js文件都会创建一个css文件, 支持按需加载css和sourceMap
  
  只能用在webpack4中, 有如下优势: 
  
    + 异步加载
    + 不重复编译, 性能更好
    + 更容易使用
    + 只针对css
    
  使用方法:
  
    1. 安装
    
      ```
      npm i -D mini-css-extract-pligin
      ```
    
    2. 在webpack配置文件中引入插件
    
      ```
      const MiniCssExtractPlugin = require('mini-css-extract-pligin')
      ```
      
    3. 创建插件对象, 配置抽离的css文件名, 支持placeholder语法
    
      ```
      new MiniCssExtractPlugin({
        filename: '[name].css'
      })
      ```
    
    4. 将原来配置的的所有*style-loader*替换为*MiniCssExtractPlugin*
      
      ```
      {
          // 用正则匹配当前访问的文件的后缀名是 .css
          test: /\.css$/,
          // webpack底层调用这些包的顺序是从右到左
          // loader的执行顺序是从右到左以管道的方式链式调用
          // css-loader 接续css文件
          // style-loader 将解析出来的文件放在html中,使其生效
          // use: ['style-loader', 'css-loader']
          use: [MiniCssExtractPlugin.loader, 'css-loader']
      },
      {
          test: /\.less$/,
          use: [MiniCssExtractPlugin.loader, 'css-loader', 'less-loader']
      },
      {
          test: /\.s(a|c)ss$/,
          use: [MiniCssExtractPlugin.loader, 'css-loader', 'sass-loader']
      },
      ```
    
## 3. 自动添加css前缀
  
  使用postcss, 需要用到 postcss-loader 和 autoprefixer 插件
    
  1. 安装 
  
    ```
     npm i -D postcss-loader autoprefixer
    ```
    
  2. 修改webpack配置文件中的loader , 将 postcss-loader 放置在css-loader的右边(调用链是从右往左)
  
    ```
    {
        // 用正则匹配当前访问的文件的后缀名是 .css
        test: /\.css$/,
        // webpack底层调用这些包的顺序是从右到左
        // loader的执行顺序是从右到左以管道的方式链式调用
        // css-loader 接续css文件
        // style-loader 将解析出来的文件放在html中,使其生效
        // use: ['style-loader', 'css-loader']
        use: [MiniCssExtractPlugin.loader, 'css-loader', 'postcss-loader']
    },
    {
        test: /\.less$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader', 'less-loader', 'postcss-loader']
    },
    {
        test: /\.s(a|c)ss$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader', 'sass-loader', 'postcss-loader']
    }
    ```
    
  3. 项目根目录下添加postcss的配置文件: postcss.config.js
  
  4. 在postcss的配置文件中使用插件
  
    ```
    module.exports = {
        plugin: [require('qutoprefixer')]
    }
    ```
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
