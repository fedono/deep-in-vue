在`vue-2.5.17-beta.0/src/core/index.js`  中 初始化 `vue` ，这时候会调用 `initGlobalAPI(Vue)` 

为什么在`vue-2.5.17-beta.0/src/core/instance/index.js` 中创建`Vue` 入口的时候使用了 `initMixin(Vue)` 但是在 `initGlobalAPI(Vue)` 中的 `initGlobalAPI` 函数中，还会再做一次的 `initMixin(Vue)` 呢？



`Vue` 所有的方法都是创建在 `Vue.prototype` 上的，如 `$watch` `$on/$off/$once` 等等 

> Vue 按功能把这些扩展分散到多个模块中去实现，而不是在一个模块里实现所有，这种方式是用 Class 难以实现的。这么做的好处是非常方便代码的维护和管理。

在 `vue-2.5.17-beta.0/src/core/observer/index.js` 

`defineReactive`   中的`Dep` 的 `target` 是什么时候加进来的？

```js
get: function reactiveGetter () {
      const value = getter ? getter.call(obj) : val
      if (Dep.target) { // 什么时候会有这个 target 
        dep.depend()
        if (childOb) {
          childOb.dep.depend()
          if (Array.isArray(value)) {
            dependArray(value)
          }
        }
      }
      return value
    }
```



## 问题
1. 什么样的数据才会被收集依赖，是在`data` 中定义的数据都会吗？ 