# Vue 中级阶段

> 学习 Vue 全家桶：Vue-CLI 、Vue-Router、Vuex

## 一、Vue CLI 简介

> 在使用 Vue 开发大型项目时，我们需要考虑代码目录结构、项目结构热部署、热加载等……因此对于这些配置如果要手动进行输入设置的话，则需要花费大量的时间。
>
> > 脚手架则是为我们的这些需求进行配置，我们只需设置该功能是否开放或者该功能需要哪些配置即可。
> >
> > > CLI：Command-Line Interface（命令行界面）脚手架
> > >
> > > vue-cli 就是为了快速搭建 Vue 开发环境以及对应的 webpack 配置而建立的脚手架。

### 1.1、脚手架的安装

- 安装 node8.9以上版本 以及 npm 包管理工具 和 webpack。
  - npm 设置淘宝镜像：`npm install -g cnpm --registry = https://registry.npm.taobao.org`
- 安装 Vue 脚手架（脚手架 3 开始需要添加/cli）：`cnpm install -g @vue/cli`
  - 脚手架中高版本是兼容低版本的，因此当我们需要调用脚手架2时，只需将 2.x 的模板进行拉取即可。
    - 安装初始化模板(包含 2 和 3 的版本)： `cnpm install @vue/cli-init -g` 
    - VueCLI2 初始化项目：`vue init webpack my-project`
    - VueCLI3 初始化项目：`vue create my-project`
- 项目初始化选项
  - ![image-20200730124828059](..\img\image-20200730124828059.png)

- node 中处理 js 的方法

![image-20200730134555486](..\img\image-20200730134555486.png)

- 项目结构解析

![image-20200730140243408](..\img\image-20200730140243408.png)

## 二、ESLint 规范

> ESLint 代码约束，为代码的规范提供规格。每次保存它都会对我们的代码进行全盘扫描，如果不符合它的标准则无法进行编译。
>
> > 爱彼得与 ESLint 标准代码规范有少许不同，但是也是提供了自身的代码规范，约束我们的代码。
> >
> > > 关闭方法：config -> index.js -> useELint 改为 false 即可。



## 三、runtimeCompiler 和 runtimeOnly 的区别

> 两个模式的区别在于 main.js 中采取的渲染方式不同。
>
> > - Vue 的工作流程：template — ast — render — virtual dom — 页面 dom
> >   - runtimeCompiler：从封装 template 开始进行逐步渲染。
> >   - runtimeOnly：直接从 render 函数形成虚拟 DOM 树后开始进行。
> >
> > ![image-20200730144704349](..\img\image-20200730144704349.png)
> >
> > <b>从执行方式来看，Only 的执行效率更高，所需代码量更少。</b>
> >
> > > render 函数执行的方式：render: h => h(App)，h 在内置函数中是以 createElement 表达，我们创建的 .vue 文件实际上类似 HTML 的表达，而 createElement 则会将这些代码转换成页面的渲染格式。
> > >
> > > - createElement
> > >
> > >   - 传入标签渲染
> > >
> > >     - ```javascript
> > >       render: function (createElement) {
> > >           // createElement("标签",{ 标签属性 },[ 标签内容 ])
> > >           return createElement("h2",{ class:'box' },[ 'Hello World' ])
> > >       }
> > >       # 编译成：<h2 class='box'> Hello World </h2>
> > >       ```
> > >
> > >   - 传入组件渲染
> > >
> > >     - ```javascript
> > >       // The Vue build version to load with the `import` command
> > >       // (runtime-only or standalone) has been set in webpack.base.conf with an alias.
> > >       import Vue from 'vue'
> > >       import App from './App'
> > >       
> > >       Vue.config.productionTip = false
> > >       
> > >       const cpn = {
> > >       	template: `<h2>Hello World</h2>`
> > >       }
> > >       
> > >       
> > >       /* eslint-disable no-new */
> > >       new Vue({
> > >         el: '#app',
> > >         // components: { App },
> > >         // template: '<App/>'
> > >       	render: h => h(cpn)
> > >       })
> > >       
> > >       ```
> > >
> > > > Only 中获取到的 `./app` 不是 template 而是被渲染的 `render 对象` ，因此可以被编译，`.vue` 文件中的 template 是由 `vue-template-compiler` 解析为 `render 对象`。
> > > >
> > > > > 上面案例是在 Compiler 模式下实现的，因此它们可以通过 template 进行编译，但是如果在 Only 中不能从 template 进行编译，所以上面案例不能在 Only 模式中实现。
> > > > >
> > > > > > 但我们用 Vue 开发时，一般都是采用 `*.vue` 的文件，因此在导入后它返回的是 `render 函数` 所以我们一般可以采用 Only 模式来开发。

## 四、dev 和 build 的执行步骤

![image-20200730153036549](..\img\image-20200730153036549.png)

![image-20200730153059588](..\img\image-20200730153059588.png)

## 五、VUE CLI3 

> VueCLI3 是渐渐流行开来的最新版本，它与 vue-cli2 有许多不同之处。
>
> - vue-cli3 由 webpack 4 打造，vue-cli2 则是 webpack 3。
> - vue-cli3 设计原则是 0 配置，移除根目录下 build 和 config 等目录。
> - vue-cli3 提供 vue ui 命令，提供了可视化配置，更加人性化。
> - 移除了 static 文件夹，新增了 public 文件夹，并且 index.html 移动至 public 中。
>
> > 具体选项
> >
> > ![image-20200730161129496](..\img\image-20200730161129496.png)

![image-20200730162409758](..\img\image-20200730162409758.png)

### 5.1、Vue-CLI3 的配置

> Vue-CLI3 为我们节省了许多配置文件，那么它的配置到底在哪里？
>
> > 为了探究 Vue-CLI3 ，Vue 提供了一个本地服务器：vue ui 来实现项目管理。运行后我们进入了一个由 vue 提供的 GUI（用户管理界面）。
> >
> > > 在导入处，导入我们的 Vue 项目后即可在界面中对项目进行管理。
> > >
> > > > 除开使用 GUI 查看配置，我们可以去 node_module 里 @vue 中 cli-service 查找配置文件。
> > > >
> > > > > 当需要自己手动改配置时，我们可以建立 vue.config.js 文件编写 `module.exports={}`，从而 vue 运行时，会将它的源文件进行合并。

## 六、箭头函数和 this 的指向

> 箭头函数可以代替 function() 的编写，从而简化我们的代码。
>
> > 箭头函数一般使用在方法中的回调函数，如：`setTimeout(3000,()=>{})`
> >
> > > 箭头函数中的 this 是指向最近作用域的 this，而 function 函数中指向的是全局。

### 6.1、箭头函数的表达

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		
		<script type="text/javascript">
			// 一般的函数定义
			const a = function a (num1,num2) {
				return num1 + num2
			}
			// ES6 箭头函数
			const b = (num1,num2) => {
				return num1 + num2
			}
			// 参数只有一个时，括号可以去掉
			const c = num => {
				return num
			}
			// 函数代码有多块代码
			const d = () => {
				console.log("a")
				console.log("b")
			}
			// 函数代码只有一块代码，返回值不需要写 return 也不需要 {} 盖起来 
			const e = (num1,num2) => num1 * num2
			const f = () => console.log("返回 undefined，但是执行 console.log")
		</script>
	</body>
</html>

```

### 6.2、箭头函数的指向

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<script>
			const a = {
				ab(){
					// 返回的是 windows 对象
					setTimeout(function(){
						console.log(this)
						// 返回的是 windows
						setTimeout(()=>{console.log(this)})
					},1000)
					// 返回的是 ab 对象
					setTimeout(()=>console.log(this),5000)
				}
			}
			a.ab()
		</script>
	</body>
</html>

```

## 七、url 的 hash 和 HTML5 的 history

> URL 的 hash 也就是锚点(#)，本质上是改变 window.location的 href 属性。我们可以直接赋值 location.hash 来改变 href，页面不发生刷新。
>
> > `location.hash = "foot"` ：跳转锚点，页面不刷新。如：localhost:8080 —> localhost:8080/#/foot
> >
> > `history.pushState({},'',foot)`：跳转锚点，页面不刷新。创建栈空间。如：localhost:8080 —> localhost:8080/foot
> >
> > `history.replaceState({},'',foot)`：跳转锚点，页面不刷新。不创建栈空间。

## 八、前端路由：vue-router 

> 路由的作用是通过互联的网络把信息从源地址传输到目的地址的活动。
>
> > - 后端渲染：jsp/php …… 数据是从后端获取而后将数据动态渲染后形成页面。
> >   - 后端路由：类似 SpringBoot 中 @Controller 与 @Service 的关系。控制器接收到前端发送的请求后形成映射，通过业务层进行逻辑处理获取数据，而后通过 Model 返回给页面进行数据渲染。（页面渲染时页面整体刷新）
> > - 前端渲染：将数据通过 ajax 请求获取数据而后将数据在前端渲染后形成页面。
> >   - 前端路由：在 SPA 中我们只有一个 HTML 页面，属于 (index.html - js - css) 结构。前端发送请求之后，会通过 指定的 JS 进行判断从而渲染不同的页面。（页面渲染时，不会整体刷新。仅替换不同部分。）

### 7.1、路由的安装及应用

> 安装：`cnpm install vue-router --save` 这是路由的安装方式，不过我们在创建脚手架时，我们可以让脚手架帮我们装好路由，这样就不需要我们自己再次手动安装。
>
> > 安装路由后，我们的 src 目录下会有 router 文件夹，里面的 index.js 就是路由的主 JS 入口。

#### 7.1.1、router-index.js 的配置

> 要配置 router 并将其应用，首先我们需要在主 JS 入口中进行配置。

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'

// 1、通过 Vue.use(插件)，安装插件
Vue.use(Router)

// 2、创建 Router 对象并导出
export default new Router({
  // 3、配置路由和组件之间的关系
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    }
  ]
})

```

### 7.2、搭建路由的映射

> 配置好路由后，我们需要对其实现映射关系，创建路由的组件，而后在配置文件中导入组件，将其放入 routers 数组中。
>
> > 在 routers 数组中，我们需要为组件指定 path 的访问域，便于前端使用 `<router-link to='path'></router-link>` 进行访问。
> >
> > > 访问成功后，Vue 会将虚拟 DOM 转换成真实 DOM 替换掉 `<router-view>` 占位标签。

```vue
<!-- App.vue 主程序入口 -->
<!-- 在主程序入口中，我们只需要布置 router 映射对象的布局即可。 -->
<!-- 在 main.js 中 render 函数是转换 App.vue 的，因此 App.vue 就被挂载到 Vue 实例中。 -->
<template>
  <div id="app">
		<!-- 定义 router-link 跳转标签，router-link 是全局组件。 -->
		<router-link to="/home">Home</router-link>
		<router-link to="/about">About</router-link>
		<!-- 根据路径渲染组件后，虚拟 DOM 会替换 router-view。 -->
		<router-view></router-view>
  </div>
</template>

<script>
export default {
  // name: 'App'
}
</script>

<style>
</style>

```

```vue
<!-- About.vue 的组件配置 -->
<template>
	<div>
		<h2>{{aboutMessage}}</h2>
	</div>
</template>

<script>
	export default {
		data(){
			return{
				aboutMessage: "Hello I'm about vue ! "
			}
		}
	}
</script>

<style>
</style>


<!-- Home.vue 的组件配置 -->
<template>
	<div>
		<h2>{{homeMessage}}</h2>
	</div>
</template>

<script>
	export default {
		data(){
			return{
				homeMessage: "Hello I'm home vue ! "
			}
		}
	}
</script>

<style>
</style>


```

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import Home from '@/components/Home'
import About from '@/components/About'


// Vue 注册 Router
Vue.use(Router)

// 将组件放入 routes 数组中
const routes = [
	{
		// 映射路径
		path: '/home',
		component: Home
	},
	{
		path: '/about',
		component: About
	},
	{
	  path: '/',
		// 第一次进入页面，重定向至 /home
		redirect: '/home'
	}
]

// 实例一个 Router 对象
export default new Router({
	// 将组件导出
  routes: routes
})

```
#### 7.2.1、更改路由的跳转模式
> 在默认情况下，路由的跳转是通过 location.hash 进行跳转的，它的地址会带有 `/#/` ，为解决这问题，我们可以在 router 配置配置文件中，设置跳转的 `mode` 模式。 

```javascript
// 实例一个 Router 对象
export default new Router({
     // 将组件导出
     routes: routes,
     // 设置跳转模式
     mode: 'history'
})
```

### 7.3、`<router-link>` 标签属性

> `<router-link>` 默认是转换成 a 标签，在开发中我们往往有其他需求，因此我们需要修改 `<router-link>` 的标签属性，对它进行配置。
>
> > - tag：指定渲染元素，可渲染成 button、div、span……等
> > - replace：默认跳转模式是 `history.pushState` ，该属性则指定为 `history.replaceState` 
> > - active-class：路由匹配成功时，路由会默认为其添加 `router-link-active，router-link-exact-active ` 的 class
> >   - 也可以在路由配置文件中，设置 `linkActiveClass` 属性，从而改变默认值。

```html
<template>
  <div id="app">
		<!-- 定义 router-link 跳转标签，router-link 是全局组件。 -->
		<router-link to="/home" tag="button" active-class="blueColor">Home</router-link>
		<router-link to="/about" tag="div" replace>About</router-link>
		<!-- 根据路径渲染组件后，虚拟 DOM 会替换 router-view。 -->
		<router-view></router-view>
  </div>
</template>

<script>
export default {
  // name: 'App'
}
</script>

<style>
.router-link-exact-active {
	color: red;
}
.blueColor{
	color: blue;
}
</style>

```



### 7.4、代码跳转路由：$router

> router 为我们进行代码跳转，为所有组件封装了一个方法 $router，便于我们使用函数控制路由的跳转。
>
> > $router 获取的是路由 JS 入口文件中的 Router 实例对象。

```html
<template>
  <div id="app">
		<button @click="getHomePage">首页</button>
		<button @click="getAboutPage">关于</button>
		<router-view></router-view>
  </div>
</template>

<script>
export default {
  // name: 'App'
	methods:{
		getHomePage(){
		  /**
			*	直接修改是无效的，因为 router 只能通过 router 来进行控制
			*	<router-link> 标签是由 router 注册的标签，所以它能够进行监听
			*/ 
			// history.pushState({},'','/home')
			
			// router 源码为 Vue 的所有组件添加了 $router 属性
			this.$router.push('/home')
			
		},
		getAboutPage(){
			this.$router.replace('/about')
		}
	}
}
</script>

<style>
.router-link-exact-active {
	color: red;
}
.blueColor{
	color: blue;
}
</style>

```

### 7.5、解决 router 跳转重复当前页面报错问题

```js
// 解决 push 复用报错问题
const VueRouterPush = Router.prototype.push 
Router.prototype.push = function push (to) {
    return VueRouterPush.call(this, to).catch(err => err)
}

// 解决 replace 复用报错问题
const VueRouterReplace = Router.prototype.replace
Router.prototype.replace = function replace (to) {
  return VueRouterReplace.call(this, to).catch(err => err)
}
```

### 7.6、动态路由：$route

> 在开发中，后端已经较多的采用 Restful 开发模式，因此我们传值时地址往往携带用户的 ID 或者某事物的 PID……如：/details/1 
>
> > - 在 Thymeleaf 开发中，我们可以通过 `/details/(id = ${...} 或 1)` 的方式进行传值。
> >
> > - 在 vue-router 中，我们可以通过类似的方法对路径进行数值定义，`path: '/details/:id'`。
> >
> >   - ```js
> >     // 将组件放入 routes 数组中
> >     const routes = [
> >     	{
> >     	  path: '/',
> >     		// 第一次进入页面，重定向至 /home
> >     		redirect: '/home'
> >     	},
> >     	{
> >     		// 映射路径
> >     		path: '/home',
> >     		component: Home
> >     	},
> >     	{
> >     		path: '/about',
> >     		component: About
> >     	},
> >     	{
> >              // 对路径进行定义，details 后缀必须要追加一个参数
> >     		path: '/details/:id',
> >     		component: Details
> >     	}
> >     ]
> >     ```

#### 7.6.1、动态路由的例子：params 获取数据

> 数据的传输是有价值的，不仅是为了作为数据的查询条件，同时也是作为渲染的内容依据。
>
> > 因此，如果我们无法获取 `:id` 那么 router 传输的价值就大大减少。为了获取由地址传输过来的数据，我们可以通过 `$route` 来获取。
> >
> > > - `$route`：指定当前被激活的路由。
> > >   - `this.$route.params`：获取路由中封装的属性，并返回列表对象。

```vue
<!-- 模拟从服务器获取数组，利用 index 代替数据 id -->
<template>
	<div>
		<h2>{{homeMessage}}</h2>
		<ul>
			<li v-for="(item,index) in messArr"><a href="javascript:;" @click="aLinkClick(index)">{{item}}</a></li>
		</ul>
	</div>
</template>

<script>
	export default {
		data(){
			return{
				homeMessage: "Hello I'm home vue ! ",
				messArr: ["Vue 的基本使用","axios 的请求方法"]
			}
		},
		methods:{
			aLinkClick(id){
				this.$router.push("/details/"+id)
			}
		}
	}
</script>

<style>
</style>
```

```vue
<!-- details 页面 -->
<template>
	<h2>大家好，我是 Details {{id.id}} 页面</h2>
</template>

<script>
	export default {
		name: "Details",
		computed:{
			id(){
				return this.$route.params
			}
		}
	}
</script>

<style>
</style>
```



### 7.7、路由懒加载

> 打包构造页面时，Javascript 包会变得非常大，影响页面加载。
>
> > 如果我们能把不同路由对应的组件进行分割成不同的块，然后当路由被访问时才加载对应组件，这样就更加高效。
> >
> > - 懒加载的方式
> >   - 方法一：结合 Vue 的异步组件和 Webpack 代码分析
> >     - ![image-20200731092218718](..\img\image-20200731092218718.png)
> >   - 方法二：AMD 写法
> >     - ![image-20200731092234855](..\img\image-20200731092234855.png)
> >   - 方法三：ES6 写法组织 Vue 的异步组件和 Webpack
> >     - ![image-20200731092309224](..\img\image-20200731092309224.png)
> >
> > > 在现在开发中，我们一般采用 ES6 写法
> > >
> > > ```javascript
> > > // 设置懒加载
> > > const Home = () => import("@/components/Home")
> > > const About = () => import("@/components/About")
> > > const Details = () => import("@/components/Details")
> > > // 将组件放入 routes 数组中
> > > const routes = [
> > > 	{
> > > 	  path: '/',
> > > 		// 第一次进入页面，重定向至 /home
> > > 		redirect: '/home'
> > > 	},
> > > 	{
> > > 		// 映射路径
> > > 		path: '/home',
> > > 		component: Home
> > > 	},
> > > 	{
> > > 		path: '/about',
> > > 		component: About
> > > 	},
> > > 	{
> > > 		path: '/details/:id/:pid',
> > > 		component: Details
> > > 	}
> > > ]
> > > 
> > > ```

### 7.8、嵌套路由

> 在路由懒加载中我们认识到一个理念，那就是我们只需要调用当前需要的东西。
>
> > 路由的嵌套就是将功能细化，比如一个分类中选择一篇 java 且 id 为 1 的文章我们可以将它细化为：`type/java/1` 或者选择一篇为 Vue 且 id 为 2 的文章：`type/vue/2` 。这样相当于在 `type` 路由中嵌套 `java` 和 `vue` 两个路由。
> >
> > > - 具体步骤
> > >   - 1、创建子路由的组件。
> > >   - 2、在路由 JS 文件中，找到需要的添加子组件的组件。为其添加 `children` 选项，并配置数组对象的 `path` 和 `component` 。
> > >   - 3、在父组件编写 `router-link to="/父组件path/子组件path"` 和 `<router-view>` 实现渲染。
> > >     - 如果父组件前不加 `/` 那么再次点击复用时，link 地址会进行累加。

```vue
<!-- Type 主分类的设置 -->
<template>
	<div>
		<ul>
			<li v-for="item in typeArr">		
				<router-link :to="'/type/'+item">{{item}}</router-link>
			</li>
		</ul>

		<router-view ></router-view>
	</div>
</template>

<script>
	export default { 
		data(){
			return{
				typeArr: ['java','python','vue']
			}
		}
	}
</script>

<style>
</style>

```

```vue
<!-- Ctype 的编写 -->
<template>
	<div>
		<h2>当前选择分类为：{{type}}</h2>
		
		<ul>
			<li v-for="item in blogArr"><a href="javascript:;" @click="aLinkClick">{{item}}</a></li>
		</ul>
		<router-link :to="/type/+currentTo"></router-link>
		<router-view></router-view>
		
	</div>
</template>

<script>
	export default {
		data(){
			return{
				blogArr: ["文章一","文章二","文章三"],
				currentTo: ''
			}
		},
		computed: {
			type(){
				return this.$route.params.ctype
			}
		},
		methods: {
			aLinkClick(){
				let pid = parseInt(Math.random()*400000+100000)
				this.$router.push("/type/"+this.type+"/"+1+"/"+pid)
				this.currentTo = this.type+"/"+1+"/"+pid
			}
		}
	}
</script>

<style>
</style>

```

### 7.9、路由的传递参数

> 在前面的案例中，我们已经实现了路由的传递参数，它是通过 `this.$route.params` 来获取路由的列表对象。
>
> > 除了采用 `params` 我们还可以采取其他方法。
> >
#### 7.9.1、`query` 截取地址数据
> 截取地址栏中 `?` 后传递的数据。该方法是通过解析：URL（协议://主机:端口/路径?查询'query'）中的 query 来实现数据获取。
> ![image-20200731105148200](..\img\image-20200731105148200.png)
> 
> >父路由的传参方式，可以是编写地址栏数据，也可以通过传递对象实现。

```vue
<template>
  <div id="app">
		<!-- 1、封装参数至地址栏的方式 -->
		<!-- <router-link to="/profile?name=gzkemays">Profile</router-link> -->
		<!-- 2、采用传参的方式 -->
		<router-link :to="profileDeafult">Profile</router-link>
		<!-- 3、采用封装方法的方式 -->
		<button @click="getProfilePage">跳转 Profile</button>		
		<!-- 根据路径渲染组件后，虚拟 DOM 会替换 router-view。 -->	
		<router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
	computed:{
		profileDeafult(){
			return {path: '/profile',query: {name: 'gzkemays',age: 18}}
		}
	},
	methods:{
		getProfilePage(){
			this.$router.replace(this.profileDeafult)
		}
	}
}
</script>
<style>
</style>

```

```vue
<template>
	<div>
		<h2>大家好，我是{{name}}。</h2>
	</div>
</template>

<script>
	export default {
		name: 'Profile',
		computed:{
			name(){
				return this.$route.query
			}
		}
	}
</script>

<style>
</style>

```

## 8.0、全局导航守卫

> 全局导航守卫用于监听我们的路由跳转，从而可以让我们可以在跳转时实现其他需求。

#### 8.0.1、实现动态更换页面 title

> 我们日常切换页面时，它显示的 title 是不同的，因此，我们可以将点击页面获取页面的内容，然后作于显示。
>
> > 因此我们需要监听路由的切换：`router.beforEach((to,from,next) => {next()})`

```js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
// import Home from '@/components/Home'
// import About from '@/components/About'
// import Details from '@/components/Details'

// 设置懒加载
const Home = () => import("@/components/Home")
const About = () => import("@/components/About")
const Details = () => import("@/components/Details")
const Type = () => import("@/components/Type")
const Profile = () => import("@/components/Profile")

// Vue 注册 Router
Vue.use(Router)

// 解决 push 复用报错问题
const VueRouterPush = Router.prototype.push 
Router.prototype.push = function push (to) {
    return VueRouterPush.call(this, to).catch(err => err)
}

// 解决 replace 复用报错问题
const VueRouterReplace = Router.prototype.replace
Router.prototype.replace = function replace (to) {
  return VueRouterReplace.call(this, to).catch(err => err)
}

// 将组件放入 routes 数组中
const routes = [
	{
	  path: '/',
		// 第一次进入页面，重定向至 /home
		redirect: '/home'
	},
	{
		name: 'Home',
		// 映射路径
		path: '/home',
		component: Home,
		meta: {
			title: '首页'
		}
	},
	{
		name: 'About',
		path: '/about',
		component: About,
		meta: {
			title: '关于'
		}
	},
	{
		name: 'Type',
		path: '/type',
		component: Type,
		meta: {
			title: 'pType'
		},
		children: [
			// 默认路径重定向选 Java
			{
				name: 'Ctype',
				path: '',
				redirect: 'java',
				meta: {
					title: 'cType'
				}
			},
			// 子路由中依旧可以嵌套子路由
			{
				path: ':ctype',
				component: () => import("@/components/Ctype"),
				meta: {
					title: 'cType'
				},
				children: [
					{
						path: ':id/:pid',
						component: () => import("@/components/Details"),
						meta: {
							title: 'detail'
						}
					}
				]
			},
		]
	},
	{
		name: 'Profile',
		path: '/profile',
		component: Profile,
		meta: {
			title: '用户'
		}
	}
]

const router = new Router({
	// 将组件导出
	routes,
	// 设置跳转模式
	mode: 'history'
})

router.beforeEach((to,from,next) => {
	// A to B ，to 监听目的路由
	if (to.meta.title == 'cType'){
		document.title = to.params.ctype
	}else if(to.meta.title == 'detail'){
		document.title = to.params.pid
	}
	// 必须填写，否则路由无法发生跳转。
	next()
	})
// 实例一个 Router 对象
export default router

```

#### 8.0.2、导航守卫的补充

> 在前面案例中，我们设置了 beforeEach 函数对 meta 进行监听判断。那么导航守卫还有其他函数供我们使用。
>
> > 在 Vue 中存在一个函数，命名为钩子函数。所谓钩子函数就是回调函数。
> >
> > > - `meta`：元数据，即对数据进行解释的数据。
> > > - 全局守卫（对全部路由实施监听）
> > >   - `beforEach(function(to,from,next){ })`：前置钩子函数，路由跳转之前进行函数回调。
> > >   - `afterEach(function(to,from){ })`：后置钩子函数，路由跳转之后进行函数回调。
> > > - 路由独享守卫（对路由实施监听）
> > >   - `beforeEnter(to,from,next) => { }`：定义在路由配置上
> > > - 组件内守卫（对路由内的组件实施监听）
> > >   - `beforeRouteEnter(to,from,next) => { }`：渲染组件的对应路由被 confirm 前调用，不能获取组件实例 this，因为守卫执行前，组件实例还没被创建。
> > >   - `beforeRouteUpdate(to,from,next) => { }`：在当前路由改变，但是该组件被复用时调用。如：`foo/1` 和 `foo/2` 两个路由它们之间有被复用的组件，这情况下该钩子函数被调用，可以访问组件的 this。
> > >   - `beforeRouteLeave(to,from,next) => { }`：导航离开该组件的对应路由时调用。可以访问实例的 this。
> > > - next 中可以附加对象列表，达到数值传输的目的。

```vue
<template>
	<div>
		<ul>
			<li v-for="item in typeArr">		
				<router-link :to="'/type/'+item">{{item}}</router-link>
			</li>
		</ul>
		<router-view ></router-view>
		
	</div>
</template>

<script>
	export default { 
		name: 'Type',
		data(){
			return{
				typeArr: ['java','python','vue'],
				cachePath: ''
			}
		},
		created() {
			console.log("type created")
		},
		destroyed() {
			console.log("type destroyed")
		},
         // 页面渲染时发生。
		activated(){
			this.$router.push(this.cachePath)
		},
         // 组件内守卫，路由每次离开都保存被激活的值。
		beforeRouteLeave(to,from,next) {
			this.cachePath = this.$route.path
			next()
		}
	}
</script>

<style>
</style>

```



 ## 8.1、vue-router-keep-alive 路由活动保留

> router-view 是一个组件，如果直接被包在 keep-alive 里，则所有路径匹配到的视图组件都会被缓存。
>
> > keep-alive 也是个组件，它是 Vue 内置的，可以使得被包含的组件保留状态，或避免重新渲染。
> >
> > > Vue 中组件都拥有自己的生命周期，当路由进行组件切换时，不同的组件将会销毁，如果我们不想让它销毁，那么我们就可以将的生命周期保存起来。
> > >
> > > ```vue
> > > <keep-alive>
> > >     <!-- 组件不会被频繁创建与销毁 -->
> > >     <router-view></router-view>
> > > </keep-alive>
> > > ```
> > >
> > > > 当组件被 keep-alive 保留起来后，它的生命周期是静止的。创建与销毁变成激活与未激活，因此保留之后，如果我们要动态调用它的状态进行回调。就要使用 `deactivated` 和 `activated` 判断是否激活。

#### 8.1.1、keep-alive 的属性

> keep-alive 是内置组件，它有两个非常重要的属性。用于决定哪个组件可以被缓存，哪个组件不能被缓存。
>
> > - 使用属性时，可以调用组件中的 name 属性来指定组件。
> >   - `include = "name1,name2"` ：字符串或正则表达式，只有匹配的组件才会被缓存。
> >   - `exclude = "name1,name2"` ：字符串或正则表达式，任何匹配的组件都不会被缓存。



