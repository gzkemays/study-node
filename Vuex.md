# Vuex

> Vuex 是一个专为 Vue.js 应用程序开发的状态管理模板。
>
> > 它采用<b>集中式存储管理</b>应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。
> >
> > > 状态管理就是将多个组件共享的变量全部存储在一个对象里，将这个对象放在顶层的 Vue 实例中，让其他组件可以使用。
> > >
> > > > 状态管理一般作用于有共享需求的组件。

- SPA 管理
- State：状态，类似 Vue 中的 data 等……
- View：视图层，可以针对 State 的变化，渲染不同的信息。
- Actions：用户的操作，如：点击、输入等……

![image-20200731162318979](..\img\image-20200731162318979.png)

## 1、Vuex 的基本应用以及配置

> Vuex 被 Vue 当作为一个仓库 store ，因此我们在配置 vuex 时，需要对它进行安装：`cnpm install vuex --save` 
>
> > 构建 store 目录，并建立 index.js 文件编写便于管理。在 index.js 文件中导入 Vue 并使其安装 Vuex 插件。
> >
> > > 配置 Vuex 的基本 options 之后将其导出，在 main.js 中获取并将其注册至 Vue 实例当中。
> > >
> > > > 配置完毕后，使用 `$store.state.*` 调用指定属性。

```js
import Vue from 'vue'
import Vuex from 'vuex'

// 安装插件
Vue.use(Vuex)

// 创建对象
const store = new Vuex.Store({
	state: {
		counter: 0
	},
	mutations: {
		
	},
	actions: {
		
	},
	getters: {
		
	},
	modules: {
		
	}
})

// 导出 store 独享
export default store
```

```js
import Vue from 'vue'
import App from './App'
import router from './router'
import store from './store'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
	// 配置 Vuex 的管理
	store,
  render: h => h(App)
})

```

### 1.1、全局单例模式（大管家）的解释

> 在上一个案例中，我们可以知道 `$store.state.counter` 的属性是共享的，它可以被任何组件直接调用。
>
> > 我们现在要把共享的状态抽取出来，进行统一管理。而后，每个视图按照规定好的规则，进行访问和修改等操作。
> >
> > > Vue 官方公布一个 Devtools 追踪观察数据是由哪个组件发生改变。
> > >
> > > > Mutations 推荐同步操作（Devtools 不支持监听异步操作），当我们需要异步操作时，则在 Actions 中进行。

![Vuex](..\img\Vuex.png)

## 2、vuex-devtools 和 mutations

> vuex-devtools 如图所示，它是监听 Mutations 的工具。Vue 在官方中推荐，我们的同步操作最好经过 Mutations 处理返回。
>
> > 如果 state 的修改不通过 Mutations 那么 devtools 无法监听到状态的改变。
> >
> > > devtools 要监听 mutations ，要在 mutations 中定义方法，对 state 中的值进行监听处理。
> > >
> > > ```js
> > > import Vue from 'vue'
> > > import Vuex from 'vuex'
> > > 
> > > // 安装插件
> > > Vue.use(Vuex)
> > > 
> > > // 创建对象
> > > const store = new Vuex.Store({
> > > 	state: {
> > > 		counter: 0
> > > 	},
> > > 	mutations: {
> > > 		increment(state){
> > > 			state.counter ++
> > > 		},
> > > 		decrement(state){
> > > 			state.counter -- 
> > > 		}
> > > 	},
> > > 	actions: {
> > > 		
> > > 	},
> > > 	getters: {
> > > 		
> > > 	},
> > > 	modules: {
> > > 		
> > > 	}
> > > })
> > > 
> > > // 导出 store 独享
> > > export default store
> > > ```
> > >
> > > > 而后主程序中通过`this.$store.commit('mutations.methods.name')` 来获取

