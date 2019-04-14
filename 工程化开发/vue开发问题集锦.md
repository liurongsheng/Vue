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

## iview-admin@1.3.1 throw new ERR_INVALID_CALLBACK(); 
```
> iview-admin@1.3.1 dev C:\Users\Administrator\Desktop\ylt-system
> webpack-dev-server --content-base ./ --open --inline --hot --port 8082 --compress --config build/webpack.dev.config.js

Happy[happybabel]: Version: 4.0.1. Threads: 8 (shared pool)
fs.js:129
  throw new ERR_INVALID_CALLBACK();
  ^

TypeError [ERR_INVALID_CALLBACK]: Callback must be a function
    at maybeCallback (fs.js:129:9)
    at Object.write (fs.js:536:14)
    at C:\Users\Administrator\Desktop\ylt-system\build\webpack.dev.config.js:12:6
    at FSReqWrap.oncomplete (fs.js:141:20)
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! iview-admin@1.3.1 dev: `webpack-dev-server --content-base ./ --open --inline --hot --port 8082 --compress --config build/webpack.dev.config.js`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the iview-admin@1.3.1 dev script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\Administrator\AppData\Roaming\npm-cache\_logs\2019-04-25T01_25_45_555Z-debug.log
```
node 版本问题，node v10 以上 fs.write 的callback 是必须的，降低Node版本可解决

将webpack.dev.config.js 和 webpack.prod.config.js 中的代码修改

`fs.write(fd, buf, 0, buf.length, 0, function(err, written, buffer) {});`

修改为： 

`fs.write(fd, buf, 0, 'utf-8', function(err, written, buffer) {});`
