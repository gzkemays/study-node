# Vue 高级阶段

> 在达到中级阶段时，我们已经几乎将 Vue 的基本用法学完了。那么高级阶段就是面向开发，我们要学会如何将 Vue 与其他 JS 或 CSS 进行整合，从而提高开发效率。

## 一、整合 Element-UI

> Element-UI 是一个适应 Vue 的 UI 前端框架，可以和 Vue 进行搭配使用。
>
> > Element-UI 提供非常简洁的界面，它可以搭配 Sass（CSS 扩展语言，可以无缝地使用任何可用的 CSS 库）。
> >
> > > 安装 element-ui：`cnpm i element-ui -S`， 安装 Sass：`cnpm install sass-loader node-sass --save-dev`

```js
import Vue from 'vue'
import App from './App'
import router from './router'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
Vue.config.productionTip = false

Vue.use(ElementUI)

/* eslint-disable no-new */
new Vue({
  el: '#app',
	router,
  render: h => h(App)
  
})

```

## 二、整合 axios

> axios 是用于处理异步请求的，与 ajax 有与其说它与 ajax 有异曲同工之妙。更不如说它和 Promise 也非常相似。
>
> > 安装：`cnpm install axios --save`
> >
> > > [具体基本使用](http://www.axios-js.com/)

### 2.1、axios 的配置

> 在开发中，axios 的配置可以方便我们的数据分类，以及数据的获取、超时等……
>
> > 配置选项，可以配置在 axios 请求中，也可以配置在默认属性中：`axios.defaults.baseURL = "http://127.0.0.1:8080"` 

#### 2.1.1、全局配置 axios.defaults.*

- `baseURL` ： 设置访问地址，如：'http://127.0.0.1:8080'
- `url`：设置具体目录，如：'/bloglist'
- `timeout`：设置超时时限 
- `transformRequest:[function(data){ }]` ：请求前的数据处理
- `transformResponse:[function(data){ }]`：请求后的数据处理
- `headers:{'x-Requsted-With':'XMLHttpRequest'}`：自定义请求头
- `params`：URL 查询对象
- `paramsSerializer:function(params){ }`：查询对象序列化函数
- `data:{key: 'aa'}`：request body
- `withCredentials:false` ：跨域是否带 Token
- `adapter:function(resolve,reject,config){ }`：自定义请求处理
- `auth:{uname: value, pwd: value2}`：身份验证信息
- `responseType:'json'`：响应的数据格式 json / blob / document / arraybuffer / text / stream

#### 2.1.2、局部配置创建实例法

> 当我们运用全局配置时会出现一个问题，就是当我们需求需要我们数据请求流量就行分流时（分服务器执行），我们不同的请求就要访问不同的服务器。
>
> > 因此我们可以创建一个 axios 的实例，来实现每次的独立访问。
> >
> > ```js
> > const instance1 = axios.create({
> > 	baseURL: 'http://123.207.32.32:8000',
> > 	timeout: 500
> > })
> > 
> > instance1({
> > 	url: '/home/multidata'
> > }).then(res => {
> > 	console.log(res)
> > })
> > ```

#### 2.1.3、抽离数据法

> 我们可以将 axios 的实例抽离出来，然后在主 js 入口中进行调用 axios，这样让我们的代码更加便于维护
>
> > ```js
> > import axios from 'axios'
> > 
> > export function request(config) {
> > 	const instance = axios.create({
> > 		baseURL: "http://123.207.32.32:8000",
> > 		timeout: 10
> > 	})
> > 	
> > 	instance(config.baseConfig)
> > 	.then( res => {
> > 		config.success(res)
> > 	})
> > 	.catch( err => {
> > 		config.failure(err)
> > 	})
> > }
> > ```
> >
> > ```js
> > const config = {
> > 	baseConfig: {
> > 		url: '/home/multidata'
> > 	},
> > 	success: function (res){
> > 		console.log(res)
> > 	},
> > 	failure: function (err){
> > 		console.log(err)
> > 	}
> > }
> > 
> > request(config)
> > ```

##### 2.1.3.1、抽离数据法之 Promise

> 数据抽离之后，我们依旧可以采用 ES6 自带的 Promise 执行异步操作。
>
> > ```js
> > export function request(config) {
> > 	return new Promise((resolve , reject) => {
> > 		const instance = axios.create({
> > 			baseURL: "http://123.207.32.32:8000",
> > 			timeout: 2000
> > 		})
> > 		instance(config)
> > 		.then( res => {
> > 			resolve(res)
> > 		})
> > 		.catch( err => {
> > 			reject(err)
> > 		})
> > 	})
> > }
> > ```
> >
> > ```js
> > /* 调用 Promise */
> > const config = {
> > 	url: '/home/multidata'
> > }
> > 
> > request(config).then(res => console.log(res)).catch(err => console.log(err))
> > ```

##### 2.1.3.2、抽离数据之直接提取法

> 我们知道，前面的例子中主要是解决实例传参问题。那么 axios create 之后相当于创建了一个 axios 对象。那么我们为了让代码更加简洁，可以在方法中直接返回创建后的对象即可。
>
> ```js
> export function request(config) {
> 	const instance = axios.create({
> 		baseURL: "http://123.207.32.32:8000",
> 		timeout: 200
> 	})
> 	return instance(config)
> }
> ```
>
> ```js
> /* 直接获取 axios 实例 */
> const config = {
> 	url: '/home/multidata'
> }
> 
> request(config).then(res => console.log(res)).catch(err => console.log(err))
> ```

### 2.2、axios 的拦截器（interceptors）

> 拦截器就是对条件进行判断访问，具体至那些可以调用，哪些不可以调用，则根据拦截器的条件进行判断。拦截用户发送的请求。 
>
> > - `instance.interceptors.request.use(config => { 拦截 success }, err => { 拦截 error })` ：拦截请求
> > - `instance.interceptors.reponse.use(config => { 拦截 success }, err => { 拦截 error })`：拦截响应
> >
> > > 拦截后，需要 return config ，否则将无法通行访问。

##  三、整合 Echarts 

> Echarts 作为一个数据可视化的手段，将数据渲染成一个表格。便于我们对数据进行观察。
>
> > 安装：`cnpm install echarts --save`
> >
> > 导入 main.js：`import echarts from 'echarts'` ，`Vue.prototype.$echarts = echarts`

## 四、整合富文本 Wangeditor