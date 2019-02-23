# Eslint校验问题

```
<p v-for="i in 20">{{i}}</p>
Elements in iteration expect to have 'v-bind:key' directives
```
解决方法：
```
v-for 后添加 :key='item'
<li v-for="i in list" :key="i">
```

```
exceeds the maximum line length of 100. (max-len)
```
解决方法：
```
法1. 设置"max-len" : ["error", {code : 300}]
法2. 使用<!-- eslint-disable --><!-- eslint-enable -->包裹

```

