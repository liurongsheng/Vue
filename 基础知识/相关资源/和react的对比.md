# Vue 和 React 局限性对比

## React的特点

react hook 常年被诟病的毛病想必大家都知道。不能在条件语句中使用，useMemo 和 useCallback 需要显式指定依赖，解决子组件重新渲染可能还需要配合 React.memo 使用等等。虽然有对应的 eslint-plugin 可以帮助填充依赖，但是依赖项一旦很多，代码的可能读会非常差。现在普遍的观点是计算量大的再用 useMemo, 而 useCallback 能不用就不用。因为这点优化对性能的影响是微乎其微的，99% 的情况下都不会出现问题，等到出现问题的时候再进行优化也不迟

## Vue的特点

### 定义状态

有 ref 和 reactive 两种方法，且 ref 在 script 标签中使用时还需要加一个 .value。虽然有了 $ref 语法糖，但最新已经废弃了。把 ref 定义的状态放在 reactive 里也要注意，否则会丢失同步。一些情况下还要区分代理对象和原始对象的不同。还有使用 console.log 打印的时候，在控制台查看很不直观。因为使用的是 Proxy 进行响应式，添加了诸如 isProxy、unref、isRef 等 api。解构 props 也会出现响应丢失的问题，还要使用 toRefs 来解决。比如想把 props 的某个属性传递给一个组合式函数时，还需要用 toRef 来保持同步。还衍生出 shadllowRef、shadllowReactive、triggerRef 等等诸多 api。

### 插槽

每个从 react 转到 vue 的人估计都会抱怨这东西的难用。因为在 react 中万物皆 props，想传什么就传什么。在 vue 中还要 具名插槽、作用域插槽 等概念。

### props 和 emits 的定义

属性 和 事件 还需要分成 defineProps 和 defineEmits 两个 api。反观 react，还是万物皆 props 的说法。props 的默认值定义值也很麻烦，需要用到 withDefaults

### 侦听 watch

watch 监听对象里的某个属性时，第一个参数还需要是一个函数。还分为好多种，watch、watchPostEffect、watchSyncEffect、watchEffect。watch 的第三个参数又有很多属性。

### 渲染函数

vue3 提供了一个 h 函数，但还是很难用，这就是为什么很多组件库都选择了 tsx 的原因。

### typescript 支持

存在语法限制。给 defineProps 定义的 ts 类型，不能从其它文件导入，只能写在这个文件里。想分开写到别的文件，只能不使用 ts 来定义类型，要使用 defineProps 的第一个参数来指定类型，这样才能从别的文件导入了。这就是为什么大多数组件库没有使用 ts 来定义类型的原因，这样定义的类型要用 ExtractPropTypes 来提取 ts 类型。

### breaking change

vue2 升级到 vue3 是不兼容的，旧项目升级是很麻烦的。但 vue2 又不是不能用，不升级就行了。等新项目再使用 vue3。反观 react，几乎是没什么影响的，但另一方面这也带来沉重的历史包袱。

## 结尾

vue3 的优点很多，但是固然也存在一些心智负担，api 之多你也可以认为这是 feature，想要记住这么多 api 并熟练使用是不容易的。人家 api 这么多都给出来了，怎么记住怎么实现功能是你们的事。仅代表个人观点，我觉得 vue3 的心智负担更重一些。你们认为 vue3 和 react 哪个对你来说心智负担更重呢，可以表达表达你们的看法。非引战，请理性讨论。
