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
   


