# Vue生命周期的理解

生命周期函数，vue实例在某一个时间点会自动执行的函数

Vue生命周期总共分为8个阶段创建前/后，载入前/后，更新前/后，销毁前/后

- beforeCreate
- created
- beforeMount
- mounted
- beforeUpdate
- updated
- beforeDestory
- destoryed

创建前/后： 在beforeCreated阶段，vue实例的挂载元素el还没有。

载入前/后：在beforeMount阶段，vue实例的$el和data都初始化了，但还是挂载之前为虚拟的dom节点，data.message还未替换。
在mounted阶段，vue实例挂载完成，data.message成功渲染。

更新前/后：当data变化时，会触发beforeUpdate和updated方法。

销毁前/后：在执行destroy方法后，对data的改变不会再触发周期函数，
说明此时vue实例已经解除了事件监听以及和dom的绑定，但是dom结构依然存在。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>lifecycle</title>
    <script src="./vue.js"></script>
</head>
<body>
    <div id="app">{{content}}</div>
    <script>
        var app = new Vue({
            el:"#app",
            data:{
                content:"liu"
            },
            beforeCreate:function(){
                console.log("beforeCreate");
            },
            created:function(){
                console.log("created");
            },
            beforeMount:function(){
                console.log(this.$el);
                console.log("beforeMount");
            },
            mounted:function(){
                console.log(this.$el);
                console.log("mounted");
            },
            beforeUpdate:function(){
                console.log("beforeUpdate");
            },
            updated:function(){
                console.log("updated");
            },
            beforeDestory:function(){
                console.log("beforeDestory");
            },
            destoryed:function(){
                console.log("destoryed");
            },
        });
        app.content="sheng";
    </script>
</body>
</html>
```
