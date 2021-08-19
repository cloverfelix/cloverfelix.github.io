# Vue


# 开始、简介

每一个Vuex应用的核心就是store（仓库）。”store“基本上就是一个容器，它包含着你的应用中绝大部分的**状态**。Vuex和单纯的全局对象有以下两点不同。

1、Vuex的状态存储是响应式的。当Vue组件从store中读取状态的时候，若store中的状态发生变化，那么相应的组件也会得到高效的更新。

2、你不能直接改变store中的状态。改变store中的状态唯一的途径就是显示的提交（commit）**mutation**。这样是得我们可以方便的跟踪每一个状态的变化。

安装完Vuex之后，我们来创建一个store

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

现在，我们可以通过`store.state`来获取状态对象，以及通过`store.commit`方法触发状态变更：

~~~js
store.commit('increment')

console.log(store.state.count) // -> 1
~~~

为了在Vue组件中访问`this.$store` property，你需要为Vue实例提供创建好的store。Vuex提供了一个从根组件向所有子组件，以`store`选项的方式注入该store的机制：

```js
new Vue({
  el: '#app',
  store: store,
})
```

为子组件或vue实例提供一个创建好的store。

现在我们可以去子组件中调用一个方法实现状态的变更：

```js
methods: {
  increment() {
    this.$store.commit('increment')
    console.log(this.$store.state.count)
  }
}
```

	注意：我们通过提交mutation的方式，而非直接改变store.state.count，是因为我们星耀更明确地追踪到状态的变化。这个简单的约定能够让你的意图更加明显，这样在阅读代码的时候能更容易地理解应用内部的状态改变。
	
	
# 核心概念

## State

Vuex使用**单一状态树**，一个对象就包含了全部的应用层级状态。至此它便作为一个”唯一数据源“而存在。这样意味着，每一个应用将仅仅包含一个store实例

存储在Vuex中的数据和Vue实例中的`data`遵循相同的规则。例如状态对象必须是纯粹 (plain) 的。

## 在Vue组件中获得Vuex状态

由于Vuex的状态存储时响应式的，从store实例中读取状态最简单的方法就是在`计算属性`中返回某个状态：

```js
// 创建一个 Counter 组件
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return store.state.count
    }
  }
}
```

每当 `store.state.count` 变化的时候，都会重新请求计算属性，并且触发更新相关联的DOM

	缺点：这种模式导致组件依赖全局状态单例。在模块化的构建系统中，在每个需要使用state的组件中需要频繁地导入。
	
Vuex通过`store`选项，提供了一种机制将状态从根组件”注入“到每一个子组件中（需要调用`Vue.use(Vuex)`）

```js
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```

通过在根实例中注册 `store` 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过 `this.$store` 访问到。让我们更新下 `Counter` 的实现：

```js
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```

## ref
