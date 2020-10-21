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
