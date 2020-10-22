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
   
   3. 将package.json中的脚本参数进行修改, 通过 *--config* 手动指定特定的配置文件
      
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
