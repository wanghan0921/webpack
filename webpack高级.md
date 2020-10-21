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
