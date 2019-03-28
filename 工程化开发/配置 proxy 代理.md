# proxy 代理

需要配置 build/webpack.dev.conf.js 的 devServer 设置

脚手架默认参数 `proxy: config.dev.proxyTable,`

所以可以在 config/index.js 配置，重新 run ,配置生效

```
dev: {
    // Paths
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {
      '/api': {
        target: 'https://dev.upaypal.cn', // 目标调用的接口域名和端口号
        changeOrigin: true, // 
        pathRewrite: {
          '^/api': '/'
        }
    }},
```

在页面调用的时候
```
methods: {
  async shareFriend() {
    const res = await this.$http.get('/api/business-platform.payment-channel.web/hqpay/gateway/activity/redEnvelope/open');
    console.log(res);
  },
},
```

浏览器调用的地址虽然还是 `http://localhost:8080/api/` 但是实际上已经经过切换
```
// General
Request URL: http://localhost:8080/api/business-platform.payment-channel.web/hqpay/gateway/activity/redEnvelope/open
Request Method: GET
Status Code: 200 OK
Remote Address: 127.0.0.1:8080
Referrer Policy: no-referrer-when-downgrade

// Request Headers
Accept: application/json, text/plain, */*
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cache-Control: no-cache
Connection: keep-alive
Host: localhost:8080
Pragma: no-cache
Referer: http://localhost:8080/
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1

```
