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
                     presets: ['@babel/env'],
                     plugins: ['@babel/plugin-syntax-class-properties', 'transform-class-properties']
                 }
             },
             // 排除js文件 , 不打包
             exclude: /node_modules/
          }
          ```
          
       babel更推荐把配置单独写一个文件 .babelrc
       
       webpack.config.js
       
       ```
       {
          test: /\.js$/,
          use: {
              loader: 'babel-loader',
              // options: {
              //     // 预设
              //     presets: ['@babel/env'],
              //     plugins: [
              //         '@babel/plugin-syntax-class-properties',
              //         'transform-class-properties',
              //         '@babel/plugin-transform-runtime'
              //     ]
              // }
          },
          // 排除js文件 , 不打包
          exclude: /node_modules/
       }
       ```
       .babelrc (json格式)
       
       ```
         {
             "presets": ["@babel/env"],
             "plugins": [
                 "@babel/plugin-syntax-class-properties",
                 "transform-class-properties",
                 "@babel/plugin-transform-runtime"
             ]
         }
       
       ```
       
         
   3. 如果需要使用[Generator](http://www.ruanyifeng.com/blog/2015/04/generator.html), 无法直接使用babel进行转换,因为会将generator转化为一个
      regeneratorRuntime, 然后使用mark和wrap来实现regenerator , 但由于babel并没有内置regeneratorRuntime , 所以无法直接使用
      
         需要安装插件 :

         *npm i @babel/plugin-transform-runtime -D*

         同时还需要安装运行时依赖:

         *npm i @babel/runtime -D*

         在.babelrc中添加插件:

         ```
         {
             test: /\.js$/,
             use: {
                 loader: 'babel-loader',
                 options: {
                     // 预设
                     presets: ['@babel/env'],
                     plugins: [
                         '@babel/plugin-syntax-class-properties',
                         'transform-class-properties',
                         '@babel/plugin-transform-runtime'
                     ]
                 }
             },
             // 排除js文件 , 不打包
             exclude: /node_modules/
         }
         ```
         
   4. 如果需要使用es6/7中对象原型提供的新方法, babel默认情况无法转换, 即使用了 *transform-runtime* 的插件也不支持转换原型上的方法
   
      需要使用另一个模块: 
      
      *npm i @babel/polyfill -S*
      
      该模块需要在使用新方法的地方直接引入
      
      ```
      import '@babel/polyfill'
      ```
   
   
## 4. source map使用

   ### 1. devtool
   
      此选项控制是否生成，以及如何生成 source map。

      使用 [`SourceMapDevToolPlugin`](/plugins/source-map-dev-tool-plugin) 进行更细粒度的配置。查看 [`source-map-loader`](/loaders/source-map-loader) 来处理已有的 source map。


## `devtool` {#devtool}

`string` `false`

   选择一种 [source map](http://blog.teamtreehouse.com/introduction-source-maps) 格式来增强调试过程。不同的值会明显影响到构建(build)和重新构建(rebuild)的速度。

   webpack 仓库中包含一个 [显示所有 `devtool` 变体效果的示例](https://github.com/webpack/webpack/tree/master/examples/source-map)。这些例子或许会有助于你理解这些差异之处。

  你可以直接使用 `SourceMapDevToolPlugin`/`EvalSourceMapDevToolPlugin` 来替代使用 `devtool` 选项，因为它有更多的选项。切勿同时使用 `devtool` 选项和 `SourceMapDevToolPlugin`/`EvalSourceMapDevToolPlugin` 插件。`devtool` 选项在内部添加过这些插件，所以你最终将应用两次插件。

   devtool                                  | 构建速度 | 重新构建速度 | 生产环境 | 品质(quality)
   ---------------------------------------- | ------- | ------- | ---------- | -----------------------------
   (none)                                   | 非常快速 | 非常快速  | yes        | 打包后的代码
   eval                                     | 非常快速 | 非常快速  | no         | 生成后的代码
   eval-cheap-source-map                    | 比较快   | 快速     | no         | 转换过的代码（仅限行）
   eval-cheap-module-source-map             | 中等     | 快速     | no         | 原始源代码（仅限行）
   eval-source-map                          | 慢      | 比较快   | no         | 原始源代码
   eval-nosources-source-map                |         |         |            |
   eval-nosources-cheap-source-map          |         |         |            |
   eval-nosources-cheap-module-source-map   |         |         |            |
   cheap-source-map                         | 比较快   | 中等     | yes        | 转换过的代码（仅限行）
   cheap-module-source-map                  | 中等     | 比较慢   | yes        | 原始源代码（仅限行）
   inline-cheap-source-map                  | 比较快   | 中等     | no         | 转换过的代码（仅限行）
   inline-cheap-module-source-map           | 中等     | 比较慢   | no         | 原始源代码（仅限行）
   inline-source-map                        | 慢      | 慢       | no         | 原始源代码
   inline-nosources-source-map              |         |         |            |
   inline-nosources-cheap-source-map        |         |         |            |
   inline-nosources-cheap-module-source-map |         |         |            |
   source-map                               | 慢      | 慢       | yes        | 原始源代码
   hidden-source-map                        | 慢      | 慢       | yes        | 原始源代码
   hidden-nosources-source-map              |         |         |            |
   hidden-nosources-cheap-source-map        |         |         |            |
   hidden-nosources-cheap-module-source-map |         |         |            |
   hidden-cheap-source-map                  |         |         |            |
   hidden-cheap-module-source-map           |         |         |            |
   nosources-source-map                     | 慢      | 慢       | yes        | 无源代码内容
   nosources-cheap-source-map               |         |         |            |
   nosources-cheap-module-source-map        |         |         |            |

  验证 devtool 名称时， 我们期望使用某种模式， 注意不要混淆 devtool 字符串的顺序， 模式是： `[inline-|hidden-|eval-][nosources-][cheap-[module-]]source-map`.

  其中一些值适用于开发环境，一些适用于生产环境。对于开发环境，通常希望更快速的 source map，需要添加到 bundle 中以增加体积为代价，但是对于生产环境，则希望更精准的 source map，需要从 bundle 中分离并独立存在。
   
   
   
 ### 2. 这么多模式用哪个比较好
   
   开发环境推荐: 
   
   *cheap-module-eval-source-map*
   
   生产环境推荐: 
   
   *(none) 不使用*
   
   原因如下: 
   
   1. 使用cheap模式可以大幅度提高soure map生成的效率 . 大部分情况下我们调试并不关心列信息, 而且就算source map 没有列 , 有些浏览器也会给出列的信息
   
   2. 使用module可以支持babel这种预编译工具, 映射转换前的代码
   
   3. 使用eval方式可大幅度提高持续构建效率. 官方文档提供的书读对比表格可以看到eval模式的重新构建速度都很快
   
   4.使用 eval-source-map 模式可以减少网络请求. 这种模式开启DataUrl 本身包含完整的 source map信息, 并不需要想 sourceUrl那样 , 浏览器需要发送一个完整的请求去获取 sourcemap 文件, 这会略微提高一点效率. 但是生产环境中就不适用了 , 因为这样会让文件变得很大
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
