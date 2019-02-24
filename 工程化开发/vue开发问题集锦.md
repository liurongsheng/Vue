# vue开发问题集锦

1. vue webpack打包后资源相对引用路径

一般情况下，通过webpack+vuecli默认打包的css、js等资源，路径都是绝对的。

但当部署到带有文件夹的项目中，这种绝对路径就会出现问题，
因为把配置的 static文件夹当成了根路径，那么要解决这种问题，就得引用相对路径。

第一种在 build/webpack.prod.conf.js 把 

找到output：修改 publicPath: './', 
```
  output: {
    publicPath: './',
    path: config.build.assetsRoot,
    filename: utils.assetsPath('js/[name].[chunkhash].js'),
    chunkFilename: utils.assetsPath('js/[id].[chunkhash].js')
  },
    
``` 

第二种在 config/index.js 把 `assetsPublicPath: '/',` 改为 `assetsPublicPath: './',` 

```
  build: {
    // Template for index.html
    index: path.resolve(__dirname, '../dist/index.html'),

    // Paths
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    assetsPublicPath: './',
      ......
  }
```
第一、二种其实是修改一个位置，推荐第二种在配置config中修改

2. vue webpack打包后.css文件里面的背景图片路径错误解决方法

```
// 期望请求
https://dev.upaypal.cn/personal/static/img/bg.1e0a6ee.jpg

// 实际请求
https://dev.upaypal.cn/personal/static/css/static/img/bg.1e0a6ee.jpg

```
资源里面的背景图片，不像index.html中加载资源一样，通过./static/js/app.js引用可以正常加载，图片资源是通过css加载的

需要修改 build/utils.js 文件，添加 `publicPath:'../../',`

```
// generate loader string to be used with extract text plugin
function generateLoaders (loader, loaderOptions) {
  const loaders = options.usePostCSS ? [cssLoader, postcssLoader] : [cssLoader]

  if (loader) {
    loaders.push({
      loader: loader + '-loader',
      options: Object.assign({}, loaderOptions, {
        sourceMap: options.sourceMap
      })
    })
  }

  // Extract CSS when that option is specified
  // (which is the case during production build)
  if (options.extract) {
    return ExtractTextPlugin.extract({
      use: loaders,
      fallback: 'vue-style-loader',
      publicPath:'../../',
    })
  } else {
    return ['vue-style-loader'].concat(loaders)
  }
}
```

