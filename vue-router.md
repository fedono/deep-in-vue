把`_route`设置成响应式的，那不该是有响应式的行为吗？行为是什么，数据设置成响应式，是当前的在其他地方用的时候，总会更新为最新的数据，但是这里的`_route`呢，在哪里会有用到呢？
`Vue.util.defineReactive(this, '_route', this._router.history.current)` 