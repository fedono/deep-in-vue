知道`vue` 是通过`defineProperty`来设置响应式，也知道是通过`set` 和`get` 来设置
但是这里面的`watch`,`target`,`depend` 的逻辑我还是没有搞清楚
```js
Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    get: function reactiveGetter () {
      const value = getter ? getter.call(obj) : val
      // TODO 这里的 Dep.target 是什么意思
      if (Dep.target) {
        dep.depend() // 这个倒是是收集当前依赖
        if (childOb) { // 然后收集子依赖
          childOb.dep.depend()
          if (Array.isArray(value)) {
            dependArray(value)
          }
        }
      }
      return value
    },
    set: function reactiveSetter (newVal) {
      const value = getter ? getter.call(obj) : val
      /* eslint-disable no-self-compare */
      if (newVal === value || (newVal !== newVal && value !== value)) {
        return
      }
      /* eslint-enable no-self-compare */
      if (process.env.NODE_ENV !== 'production' && customSetter) {
        customSetter() // 这个不知道啥意思
      }
      if (setter) {
        setter.call(obj, newVal)
      } else {
        val = newVal
      }
      childOb = !shallow && observe(newVal)
      dep.notify() // 这个就是触发更新了
    }
  })
```

关于 `Dep.target`的问题，在 `src/core/observer/dep.js` 中
也就是执行 `pushTarget` 的时候会进行设置
> 难道说，只有`watcher` 的属性，才会有 `Dep.target`这个吗？不是吧，不是 data 中的数据都会被手机依赖吗？
```js
// the current target watcher being evaluated.
// this is globally unique because there could be only one
// watcher being evaluated at any time.
Dep.target = null
const targetStack = []

export function pushTarget (_target: ?Watcher) {
  if (Dep.target) targetStack.push(Dep.target)
  Dep.target = _target
}

export function popTarget () {
  Dep.target = targetStack.pop()
}
```
在`src/core/observer/watcher.js` 中，有`pushTarget` 的调用
```js
/**
* Evaluate the getter, and re-collect dependencies.
*/
get () {
    pushTarget(this)
    // ...
}
```


有对当前`vm._watcher`的设置
```js
if (isRenderWatcher) {
  vm._watcher = this
}
vm._watchers.push(this)
```

