# webpack loader

## 1. 处理css

   1. 安装 *npm i css-loader style-loader -D*
   
   2. 配置 webpack.config.js
   
   ```
   // loader配置
    module: {
        rules: [
            // 配置的是用来解析.css文件的loader (css-loader 和 style-loader)
            {
                // 用正则匹配当前访问的文件的后缀名是 .css
                test: /\.css$/,
                // webpack底层调用这些包的顺序是从右到左
                // loader的执行顺序是从右到左以管道的方式链式调用
                use: ['style-loader', 'css-loader']
            }
        ]
    }
   ```
   
   注释 : 
     
   * css-loader * : 解析css文件
     
   * style-loader * : 将解析出来的文件放在html中,使其生效
   
 
 ## 2. 处理 less 和 sass 
 
   1. 安装 *npm i less less-loader -D*
   
   2. 安装 *npm i node-sass sass-loader -D*
   
   ```
      {
          test: /\.less$/,
          use: ['style-loader', 'css-loader', 'less-loader']
      },
      {
          test: /\.s(a|c)ss$/,
          use: ['style-loader', 'css-loader', 'sass-loader']
      }
   ```
   
 ## 3. 处理 图片 和 字体
 
   1. 安装 *npm i file-loader -D*
   
   ```
   {
       test: /\.(png|jpg|gif)$/,
       use: 'file-loader'
   },
   {
       test: /\.(woff|svg|woff2|ttf|eot)$/,
       use: 'file-loader'
   }
   ```
  
  2. 更进一步 安装 *npm i url-loader -D*
  
     > 安装 url-loader 之前, 需要先安装 file-loader

   ```
   {
       test: /\.(png|jpg|gif)$/,
       use: {
           loader: 'url-loader',
           options: {
               // 图片小于5kb时, 会转化成base64
               // base64的体积会比原图大30%左右 , 但是base64不用额外请求 , 所以当图片体积较小(一般不超过5kb)时 , 转成base64格式
               // 图片体积大于 5kb 时 , 就以路径的形式展示
               limit: 5 * 1024,
               // 打包后 图片存放的文件夹
               outputPath: 'images',
               // 图片命名 , hash 代表用hash算法
               name: '[name]-[hash:4].[ext]'
           }
       }
   },
   ```
   
   ## 4. babel
   
   1. *npm i babel-loader @babel/core @babel/preset-env webpack -D*
   
       @babel/core babel 核心
       
       @babel/preset-env 预设
      
   2. 如果需要支持更高级的ES6语法 , 可以继续安装插件:
      
         *npm i @babel/plugin-proposal-class0properties -D*
         
         也可以根据需要在babel官网找插件进行安装
         
    ```
    {
       test: /\.js$/,
       use: {
           loader: 'babel-loader',
           options: {
               // 预设
               presets: ['@babel/env']
           }
       }
    }
    ```
