销毁的过程中，父组件是在子组件中销毁之后再销毁的
在 `Vue.prototype.$destroy` 中有以下判断
```js
// 组件销毁的过程中，如果父组件存在，并且父组件不是正在销毁，那么就开始销毁父组件的子组件

// 哦上面说的应该不对，这里只是把自己从父组件中销毁
// remove self from parent
const parent = vm.$parent
if (parent && !parent._isBeingDestroyed && !vm.$options.abstract) {
  remove(parent.$children, vm)
}
```

在`src/core/observer/scheduler.js` 中看到官方说明，父组件创建肯定是在子组件前的
> Components are updated from parent to child. (because parent is always created before the child)


面作业帮的时候问过一个问题
在 data 中设置的变量，会在 `mounted` 中被依赖收集吗？
答案是会，只要是在`data`中设置的数据，都会被依赖收集
```js
// initData 中，最后一步，就是把所有的data中的数据设置为响应式
// observe data
observe(data, true /* asRootData */)
```