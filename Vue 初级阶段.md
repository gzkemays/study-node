# Vue 初级阶段

## 一、Vue 的简介

- Vue 是一套用于构建用户界面的渐进式框架，Vue 被设计为可以自底向上逐层应用。
- Vue 的数据驱动是通过 MVVM（Model-View-ViewModel）模式来实现的，其基本工作原理：

![image-20200713162902918](..\img\image-20200713162902918.png)

- Vue 提供了 ViewModel 的工作机制，相当于为视图与数据提供一个交互平台。
  - 视图引导数据： View 发生变化时，通过 DOM Listeners 来通知 Model 进行修改。
  - 数据引导视图： Model 发生变化时，通过 Data Bindings 来通知 View 进行修改。
  - 通过以上 ViewModel 的工作机制，实现了视图与模型的互相解耦。
- Vue 使用基于依赖追踪的观察系统并且使用异步队列更新，所有的数据都是独立触发的，提高了数据处理能力。
- React 和 Vue 的中心思想是：一切都是组件，组件之间可以实现嵌套。
  - React 采用了特殊的 JSX 语法。
  - Vue 推崇编写以 *.vue 后缀命名的文件格式，对文件内容都有一些规定。
  - 两者需要编译后使用。

- React 依赖虚拟 DOM，而 Vue 使用的是 DOM 模板。
  - Vue 在模板中提供了指令、过滤器等，可以非常方便和快捷地操作 DOM。所以推荐将 Vue 使用到具有复杂交互逻辑的前端应用中，以确保用户的体验效果。

## 二、安装

### 三种方法

- 进入[官网下载地址](https://cn.vuejs.org/v2/guide/installation.html)选择
  - 开发版本(包含完整的警告和调试模式)
  - 生产版本(删除了警告，33.30KB min+gzip)
- 调用 CDN 
  - ```<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>``` 调用最新版本
  - ```<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>``` 指定版本号
- 使用 NPM：
  - ```npm install npm```

## 三、初步认识 Vue

- Vue 的引用以及实例化
  - const：定义常量
  - let：定义变量
  - var：定义全局属性
- Vue 的编程范式是声明式的，与原来的命令式编程有所不同。
  - 命令式编程：通过代码的编写改变元素的属性。
  - 声明式编程：通过代理的方式声明元素的属性。

### 3.1、调用 Vue 的简单示例（el 和 data）

- 实例化 Vue 对象：定义对象获取 Vue 实例：const vm = new Vue({})。
- el：指定挂载管理的元素，通常获取指定的 id（唯一元素）当然若有需要，获取指定的 class 也可以。
- data：定义初始化数据，数据的来源可以源自服务器、网络，也可以开发者自己定义。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			{{message}}
		</div>	
        
	<script src="../js/vue.js" type="text/javascript" charset="utf-8"></script>
	<script>
		const vm = new Vue({
			// 用于挂载管理的元素
			el: "#app",
			// 定义初始化数据
			data: {
				message: "Hello Vue"
			}	
		})
	</script>
	</body>
</html>
```

### 3.2、遍历循环生成（v-for）

- 如果获取一个数组的数据并需要将它显示出来，那么我们就需要对该数组进行遍历。
- JavaScript 中遍历获取元素：`for( a in b)`，for 循环是执行在 script 当中的。
- Vue 中遍历获取元素：`<div v-for='a in b'>{{a}}</div>`，for 循环是执行在 DOM 当中的。
  - Vue 在处理这数据方面与 Thymeleaf 有异曲同工之妙。
    - 同样，`v-for` 可以使用`(item,index) in list ` 这样来获取它数组元素的索引值。
  - 它们都是先获取数据，后在 DOM 元素属性中做数据的处理，从而控制 DOM 元素的显示效果。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<ol>
				<li v-for="item in list">{{item}}</li>
			</ol>
             <!-- 通过 index 获取数组元素的索引值 -->
			<div v-for="(item,index) in list">
				{{index+1}} --- {{item}}
			</div>
		</div>
		
		<script src="../js/vue.js" type="text/javascript" charset="utf-8"></script>
		<script>
			const vm = new Vue({
				// 用于挂载管理的元素
				el: "#app",
				// 定义初始化数据
				data: {
					list: [1,2,3,4,5]
				}
				
			})
		</script>
	</body>
</html>

```

### 3.3、自定义方法实现计数器（v-on:click/@click 和 methods）


- 计数器就是通过 “+” 或 “-” 来实现数字的增加或减少。
- JavaScript的实现：在 <font color='red'>script</font> 中绑定按钮的 click 事件实现 function 将初始化数据 currentData ++，
- Vue 的实现：在 <font color='red'>DOM 标签属性</font>中绑定按钮的 click 事件实现代码逻辑 — currentData++。
  - 对于简单的代码逻辑的实现我们可以定义在元素标签中。当对于复杂的代码逻辑，我们可以定义 function。
  - 在 Vue 中，function 将存储在 methods 当中，从而 methods 成为一个函数库，用于给 Vue 指定的容器调用。
    - 方法：指定义在函数里面的代码逻辑。（类似于 Java 中类里编写的方法）
    - 函数：指定义在直接定义在外部的代码逻辑。（类似于一个 Java 类）
  - Vue 中 `v-on` 相当于 jQuery 中的 `$("button").on("click",function(){})` 的操作，用于绑定 DOM 元素中的事件。
    - 若属性在 Vue 中已有定义的可以直接调用。
    - @ 是 `v-on` 的语法糖（简写）。
  - 点击事件的发生实际上就是 视图引导数据 —> 数据引导视图 的一个 MVVM 的实现。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<h1>当前计数:{{currentData}}</h1>
			<h2>在属性中实现简单的代码逻辑</h2>
			<!-- 在 DOM 元素属性中实现 ++ 和 -- -->
			<button type="button" @click="currentData++">+</button>
			<button type="button" @click="currentData--">-</button>
			<hr>
			
			<h2>调用 Vue 中定义的 methods</h2>
			<!-- 在 DOM 元素属性中执行 Vue 定义好的 methods - function -->
			<button type="button" @click="add()">+</button>
			<button type="button" @click="subtraction()">-</button>
		</div>
		
		
		<script src="../js/vue.js" type="text/javascript" charset="utf-8"></script>
		<script>
			const vm = new Vue({
				// 用于挂载管理的元素
				el: "#app",
				// 定义初始化数据
				data: {
					currentData: 0
				},
				// 定义方法
				methods:{
					add: function(){
						// data 中定义的数据在 script 中不是全局的，因此我们需要用 this 来声明当前对象。
						this.currentData++
						alert("加加成功！")
					},
					subtraction: function(){
						this.currentData--
						alert("减减成功！")
					}
				}
				
			})
		</script>
	</body>
</html>

```

#### 3.3.1、将外部定义的数据嵌入 Vue 对象的 Data 中

- 在 Vue 的外部，我们可以定义一个数据，将它嵌入至 data 当中成为 Vue 的一部分。
- 当嵌入后，不需要通过 `this.myData.**` 来实现数据调取，直接 `this.**` 即可。

```html
		<script src="../js/vue.js" type="text/javascript" charset="utf-8"></script>
		<script>
			const myData = {
				currentData : 0
			}	
			const vm = new Vue({
				// 用于挂载管理的元素
				el: "#app",
				// 定义初始化数据
				data: myData,
				// 定义方法
				methods:{
					add: function(){
						// data 中定义的数据在 script 中不是全局的，因此我们需要用 this 来声明当前对象。
						this.currentData++
						alert("加加成功！")
					},
					subtraction: function(){
						this.currentData--
						alert("减减成功！")
					}
				}
				
			})
		</script>
```

## 四、Vue 的生命周期

- Vue 和其他框架相同，它也有属于自己的声明周期。
  - Vue 实例创建之前：beforeCreate(){}
  - Vue 实例创建之后：created(){}
  - Vue 实例对页面挂载之前：beforeMount(){}
  - Vue 实例对页面挂载之后：mounted(){}
  - Vue 实例更新 DOM 元素之前：beforeUpdate(){}
  - Vue 实例更新 DOM 元素之后：updated(){}
  - Vue 实例销毁之前：beforeDestroy(){}
  - Vue 实例销毁之后：destroyed(){}

### 4.1、利用声明周期实现阶段性方法的简单实例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Learn9</title>
    <script src="js/vue.js"></script>
</head>
<body>
    <div id = "app">
        <h3>测试实例创建</h3>
        {{msg}}
        <hr>
        <h3>测试实例挂载</h3>
        {{mount_msg}}
        <hr>
        <h3>测试实例更新</h3>
        <button @click="update_judge = !update_judge">update</button>
        <!-- ref 定义 DOM 组件的名称，便于指定获取。  -->
        <div v-if="update_judge" ref="self">{{update_msg}}</div>
        <hr>
        <h3>测试实例销毁</h3>
        <button @click="destroyMean">点击我销毁 Vue</button>
        <div ref="destroy_self">我是测试被销毁的 div 请查看控制台数据显示</div>
        {{destroy_msg}}
    </div>
    <script>
        const vm = new Vue({
            el:"#app",
            data:{
                msg : "我是被实例测试的 msg",
                mount_msg:"我是被挂载测试的 msg",
                update_msg:"我是被更新测试的 msg",
                update_judge:false,
                destroy_msg:"我是被销毁测试的 msg 请查看控制台数据显示"
            },
            methods:{
                destroyMean(){
                    vm.$destroy()
                }
            },
            // Vue 实例创建之前 data 的初始化数据未被 Vue 实例化。
            beforeCreate() {
                console.log("1、实例创建之前的 msg :"+this.msg)
                // alert("实例创建之前的 msg :"+this.msg)
            },
            // Vue 实例创建之后 data 的初始化数据被 Vue 实例化。
            created() {
                console.log("1、实例创建成功后的 msg :"+this.msg)
                // alert("实例创建成功后的 msg :"+this.msg)
            },
            // Vue 实例对前端页面进行挂载之前，前端页面并不能显示调用 Vue 实例
            beforeMount() {
                console.log("2、实例挂载之前的 mount_msg :"+this.$el.innerHTM)
                // alert("实例挂载之前的 mount_msg :"+this.$el.innerHTML)
            },
            // Vue 实例对前端页面进行挂载之后，前端页面可以显示并调用 Vue 实例
            mounted() {
                console.log("2、实例挂载之后的 mount_msg :"+this.$el.innerHTML)
                // alert("实例挂载之后的 mount_msg :"+this.$el.innerHTML)           
            },
            // Vue 实例更新之前绑定的 DOM 元素并不能被 Vue 的 $refs 获取因此 innerText undefined
            beforeUpdate() {
                console.log("3、实例更新之前 :"+this.$refs.self.innerText)
                // alert("实例更新之前 :"+this.$refs.self.innerText)
            },
             // Vue 实例更新之后绑定的 DOM 元素被 Vue 的 $refs 获取
            updated() {
                console.log("3、实例更新之后被 $refs 获取 :"+this.$refs.self.innerText)
                // alert("实例更新之后被 $refs 获取 :"+this.$refs.self.innerText)               
            },
            // Vue 实例销毁之前
            beforeDestroy() {
                // alert("vue 实例销毁之前的 div :"+this.$refs.destroy_self)
                console.log("4、vue 实例销毁之前的 msg :"+this.destroy_msg+"—————— 销毁之前的 $refs 获取的 DOM 元素"+this.$refs.destroy_self)  
            },
            destroyed() {
                // alert("vue 实例销毁之后的 div :"+this.$refs.destroy_self) 
                console.log("4、vue 实例销毁之后的 msg :"+this.destroy_msg+"—————— 销毁之前的 $refs 获取的 DOM 元素"+this.$refs.destroy_self)             
            },
        })
    </script>
</body>
</html>
```

## 五、认识 Mustache

> Mustache(胡子)，在 Vue 中 Mustache 的意思就是大括号。用于将 Vue 中定义的数据表达在 HTML 上。
>
> > ​	在括号内我们可以任意以 String 的形式结合 Vue 中定义的数据。也可以分开进行结合。
> >
> > ```html
> > <!DOCTYPE html>
> > <html>
> > 	<head>
> > 		<meta charset="utf-8">
> > 		<title></title>
> > 	</head>
> > 	<body>
> > 		<div id="app">
> > 			<h3>{{message}},{{prefix}}</h3>
> > 			<h3>{{message + ',' + prefix}}</h3>
> > 			
> > 		</div>
> > 		<script src="../js/vue.js"></script>
> > 		<script>
> > 			const htmlData = {
> > 				message: "Hello world",
> > 				prefix: "My name is Vue"
> > 			}
> > 			const app = new Vue({
> > 				el: "#app",
> > 				data: htmlData
> > 			})
> > 		</script>
> > 	</body>
> > </html>
> > ```

## 六、Vue 的插值操作

- Vue 的其他指令（指令：类似 Java 的 void 方法，直接调用不需要配置参数、表达式）

  - `v-once` 指定该 DOM 元素只触发/渲染一次，用 v- 定义时，页面渲染成功后当作触发。若指定`例：@click.once`相对事件时，则事件执行后当作触发。

    - ```html
      <!DOCTYPE html>
      <html>
      	<head>
      		<meta charset="utf-8">
      		<title></title>
      	</head>
      	<body>
      		<div id="app">
      			<!-- 定义了页面首次渲染时触发唯一事件，那么它将不可被修改。-->
      			<div v-once>{{message}}</div>
      			<!-- 可被修改-->
      			<div>{{message}}</div>
      			<button type="button" @click="message = 'Hellow World!' ">点击我修改 Message</button>
      		</div>
      		<script src="../js/vue.js"></script>
      		<script>
      			const app = new Vue({
      				el: "#app",
      				data: {
      					message: "Hellow Vue!"
      				}
      			})
      		</script>
      	</body>
      </html>
      
      ```

  - `v-html` 指定该 DOM 元素为标签，用于在 data 中定义的数据为 `<a></a>、<div></div>` 等标签的内容时，直接用 Mustache 是默认是 text 属性的，若要将 data 中数据转换成 HTML 标签则可以在 DOM 元素属性中添加 `v-html = 'message'`  将 `message` 内容转换成 HTML 内容。

    - ```html
      <!DOCTYPE html>
      <html>
      	<head>
      		<meta charset="utf-8">
      		<title></title>
      	</head>
      	<body>
      		<div id="app">
      			<!-- Vue 使用 Mustache 传输表达的数据默认为 text -->
      			<div>
      				{{aDiv}} + {{aHref}}
      			</div>
      			<!-- v-html 属性可以将传输的数据转换为 html -->
      			<div v-html="aDiv + aHref"></div>
      		</div>
      		<script src="../js/vue.js"></script>
      		<script>
      			const app = new Vue({
      				el: "#app",
      				data:{
      					aDiv: "<div>Hello Vue!</div>",
      					aHref: "<a href='https://wwww.baidu.com'>百度一下</a>"
      				}
      			})
      		</script>
      	</body>
      </html>
      ```

  - `v-text` 它与 `v-html` 类似，由于 Vue 默认传输的数据属性是 Text 类型，那么 `v-text` 的作用就是代替 Mustache 表达式。可以直接将数据嵌在 DOM 元素的属性上。

  - `v-pre` 将 data 数据原封不动的显示出来，不会进行数据解析。

    - ```html
      <!DOCTYPE html>
      <html>
      	<head>
      		<meta charset="utf-8">
      		<title></title>
      	</head>
      	<body>
      		<div id="app">
      			<!-- 数据解析成 Hello World -->
      			<div>{{message}}</div>
      			<!-- 数据不进行解析，输出 {{message}}-->
      			<div v-pre>{{message}}</div>
      		</div>
      		<script src="../js/vue.js"></script>
      		<script>
      			const app = new Vue({
      				el: "#app",
      				data: {
      					message: "Hello World!"
      				}
      			})
      		</script>
      	</body>
      </html>
      ```

    - `v-cloak` 控制 Vue 渲染前的样式，可以在 style 中通过 `[v-cloak]{display: none}` 的方式定义属性样式。当 Vue 渲染成功后 `v-cloak` 会自动消失。

      - ```html
        <!DOCTYPE html>
        <html>
        	<head>
        		<meta charset="utf-8">
        		<title></title>
        		<style>
        			/* 定义 v-cloak 的样式，当 Vue 渲染成功前执行 */
        			[v-cloak] {
        				display: none;
        			}
        		</style>
        	</head>
        	<body>
        		<!-- v-cloak 会在 Vue 渲染成功后自动去掉 -->
        		<div id="app" v-cloak>
        			<div>{{message}}</div>
        		</div>
        		<script src="../js/vue.js"></script>
        		<script>
        			setTimeout(function(){
        				const app = new Vue({
        					el: "#app",
        					data: {
        						message: "Hello Vue",
        					},
        				})
        				},1000)
        		</script>
        	</body>
        </html>
        ```

## 七、动态数据的绑定

> 在正常开发中，大部分数据它都不是写死的，都是动态获取的，因此我们需要对数据进行动态绑定。
>
> > 在 jQuery 中我们采用命令式的数据绑定，它是将获取的数据后采用 val、attr、text、html 等命令改变 DOM 元素的值。
> >
> > 在 Vue 中则可以通过类似 Thymeleaf 的数据绑定的方式，在 DOM 元素的属性中进行交互绑定。 

### 7.1、v-bind 数据的绑定

- Vue 的 `v-bind` 与 Thymeleaf 的 `th` 有点类似，都是通过 `mean: key = value` 的方式对属性进行绑定。 

  - `v-bind:src`相当于 Thymeleaf 的 `th:src` ，`href`也相同。

  - `v-bind` 也有个语法糖，也就是可以忽略 `v-bind 直接采用 : 的形式表示`。

  - ```html
    <!DOCTYPE html>
    <html>
    	<head>
    		<meta charset="utf-8">
    		<title></title>
    	</head>
    	<body>
    		<div id="app">
    			<img v-bind:src="imgURL" >
    			<a v-bind:href="aLink">v-bind</a>
    			<!-- 语法糖省略 v-bind -->
    			<img :src="imgURL" >
    			<a :href="aLink">v-bind:href</a>
    		</div>
    		<script src="../js/vue.js"></script>
    		<script>
    			const app = new Vue({
    				el: "#app",
    				data: {
    					imgURL: "https://picsum.photos/id/1/200/300",
    					aLink: "https://www.baidu.com"
    				}
    			})
    		</script>
    	</body>
    </html>
    ```

### 7.2、v-bind 对象语法实现动态添加 class

- 开发中，有时会遇到鼠标点击、悬停等事件添加或消除 css 的需求。

- jQuery 中可以采用 `$("#id").on('hover',function(){})` 的命令式的方法添加 `function`切换 class 的代码逻辑。

- Vue 中则可以直接在 DOM 元素中使用 `v-bind:class` 对 class 进行切换。

  - `v-bind:class="{key1:value1 , key1:value1}"` 通过键值对进行判断，然后执行是否添加 class。

    - `value`值主要是通过 Vue 中的实体类中定义的 `data` 内获取。

  - 在属性内进行键值对判断的方式，叫作对象语法，它会根据对象进行判断是否添加样式。

  - ```html
    <!DOCTYPE html>
    <html>
    	<head>
    		<meta charset="utf-8">
    		<title></title>
    	</head>
    	<style type="text/css">
    		.active{
    			color:red;
    		}
    	</style>
    	<body>
    		<div id="app">
    			<!-- 根据 isActive 的布尔值来判断是否添加 active 样式 -->
    			<h2 :class="{active: isActive}">{{message}}</h2>
    			<!-- 也可以通过 methods 中定义的方法的返回值进行直接写入 -->
    			<h2 :class="getClasses()">{{message}}</h2>
    			<!-- 绑定 conActive 切换 isActive 的布尔值 -->
    			<button type="button" @click="conActive">改变字体颜色</button>
    		</div>
    		<script src="../js/vue.js"></script>
    		<script>
    			const testData = {
    				isActive: true,
    				message: "Hello world!"
    			}
    			const app = new Vue({
    				el: "#app",
    				data: testData,
    				methods: {
    					conActive: function(){
    						this.isActive = !this.isActive
    					},
    					getClasses: function(){
    						return {active: this.isActive}
    					}
    				}
    			})
    		</script>
    	</body>
    </html>
    ```

### 7.3、v-bind 数组语法实现批量添加 class

- 我们如果需要一次性导入多个 class 时，我们可以通过数组语法的方法，一次性将数组里的内容转换成 class 字符串。

- 这种方法添加的数据是动态的，可修改的。

  - class 的数组里的值必须是从 Vue 对象中定义的，不能直接读取 style 中定义好的别名。

- ```html
  <!DOCTYPE html>
  <html>
  	<head>
  		<meta charset="utf-8">
  		<title></title>
  	<style>
  		.myfont{
  			color: blue;
  		}
  	</style>
  	</head>
  	<body>
  		<div id="app">
  			<cpn></cpn>
  		</div>
  		
  		<!-- 子组件 -->
  		<template id="cpn">
  			<div>
  				<!-- 定义绑定数据的名称，并指定绑定的数据。 -->
  				<slot v-bind:efg="messArr" :a="color" :class="[myClass]">
  					<ul>
  						<li v-for="item in messArr">{{item}}</li>
  					</ul>
  				</slot>
  			</div>
  		</template>
  
  		<!-- 父组件 -->
  		<template id="p_cpn">
  			<div>
  				<h2>我是父组件</h2>
  				<cpn></cpn>
  				<cpn>
  					<!-- 创建 v-slot 并起别名 -->
  					<template v-slot="abc">
  						<div>
  							<!-- 通过别名获取子组件的 slot 绑定属性数据 -->
  							<!-- abc.class 调用无效 -->
  							<span :style="{color: abc.a}" :class="[myClass]">{{abc.efg.join(" - ")}} {{abc.class}}</span>
  						</div>
  					</template>
  				</cpn>
  			</div>
  		</template>
  		
  		<script src="../js/vue.js"></script>
  		<script>
  			const cpn = {
  				template: '#cpn',
  				data(){return {
  					messArr:["子组件数据1","子组件数据2","子组件数据3"],
  					color: 'red',
  					myClass: "myfont"
  				}}
  			}
  			
  			const p_cpn = {
  				template: '#p_cpn',
  				data(){
  					return{
  						myClass: "myfont"
  					}
  				},
  				components:{
  					cpn: cpn
  				}
  			}
  			
  			const app = new Vue({
  				el: "#app",
  				components:{
  					cpn: p_cpn
  				}
  			})
  		</script>
  	</body>
  </html>
  
  ```

### 实操：实现点击切换样式

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<style type="text/css">
		ul{
			list-style: none;
		}
		li{
			float: left;
			margin-left: 50px;
		}
		li:hover{
			cursor: pointer;
		}
		.active{
			color: red;
		}
	</style>
	<body>
		<div id="app">
			<ul>
                 <!-- 在 class 中定义逻辑：当 isItem == 当前索引值 index 时，在当前 DOM 元素中样式 -->
				<li v-for="(movie,index) in movies" @click="changeClass(index)" :class="{active: isItem == index}">{{index+1}} — {{movie}}</li>
			</ul>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData = {
				movies:["海王","肖生克的救赎","那一年我们错过的女孩","飞驰人生"],
				isItem: 0
			}
			
			const app = new Vue({
				el: "#app",
				data:testData,
				methods:{
					changeClass:function(itemIndex){
						this.isItem = itemIndex
					}
				}
			})
		</script>
	</body>
</html>
```

### 7.4、v-bind 对象语法实现动态添加 style

> 动态添加 style 与 动态添加 class 的操作方法类似，主要通过 `key:value` 值进行传递。
>
> ```html
> <!DOCTYPE html>
> <html>
> 	<head>
> 		<meta charset="utf-8">
> 		<title></title>
> 	</head>
> 	
> 	<body>
> 		<div id="app">
> 			<div :style="{fontSize:fSize + 'px',backgroundColor:bColor}" style="border: red 1px solid;height: 200px;width: 200px;">{{message}}</div>
> 			<button type="button" @click="fSize<=10 ? fSize = fSize-0 : fSize = fSize-10">字体变小</button>
> 			<button type="button" @click="fSize>=10 && fSize<100 ? fSize = fSize+10 : fSize = fSize+0">字体变大</button>
> 			<button type="button" @click="bColor == 'red' ? bColor='blue' : bColor='red' ">背景变色</button>
> 			
> 		</div>
> 		<script src="../js/vue.js"></script>
> 		<script>
> 			const testData = {
> 				fSize: 100,
> 				bColor: 'red',
> 				message: 'Hello Vue ! '
> 			}
> 			const app = new Vue({
> 				el: "#app",
> 				data: testData
> 			})
> 		</script>
> 	</body>
> </html>
> 
> ```

### 7.5、v-bind 数组语法实现动态添加 style

> 该方法基本不用，所以此处只做示例。
>
> >```html
> ><!DOCTYPE html>
> ><html>
> >	<head>
> >		<meta charset="utf-8">
> >		<title></title>
> >	</head>
> >	<body>
> >		<div id="app">
> >			<div :style="[bgColor,bgHeight,bgWidth]"></div>
> >		</div>
> >		<script src="../js/vue.js"></script>
> >		<script>
> >			const testData = {
> >				bgColor: {backgroundColor: "red"},
> >				bgHeight: {height: "100px"},
> >				bgWidth: {width: "100px"}
> >			}
> >			const app = new Vue({
> >				el: "#app",
> >				data: testData
> >			})
> >		</script>
> >	</body>
> ></html>
> >
> >```

## 八、计算属性 computed 的使用

> 计算属性的作用：在 data 中属性是不可以相互进行计算的，若要进行属性间的计算，就要在 computed 中定义一个新的属性，从而返回属性的计算结果。
>
> > `computed` 与 `methods` 的不同之处是，`computed` 多次调用实际上只会调用一次相对复杂度是O(1)拥有内部缓存，而`methods`则是一样进行多次调用相对复杂度是O(n) 。
>
> > > 计算属性是一个新的属性，它执行后会返回给 vue 对象实例当中充当属性的角色。
> > >
> > > ```html
> > > <!DOCTYPE html>
> > > <html>
> > > 	<head>
> > > 		<meta charset="utf-8">
> > > 		<title></title>
> > > 	</head>
> > > 	<body>
> > > 		<div id="app">
> > > 			<div>{{dataAllNum}}</div>
> > > 			<div>{{allNum}}</div>
> > > 			<div>{{testAllNum()}}</div>
> > > 			<div>{{booksPrice}}</div>
> > > 		</div>
> > > 		<script src="../js/vue.js"></script>
> > > 		<script>
> > > 			const testData = {
> > > 				oneNum: 1,
> > > 				twoNum: 2,
> > > 				threeNum: 3,
> > > 				dataAllNum: parseInt(this.oneNum)+parseInt(this.twoNum)+parseInt(this.threeNum),
> > > 				books: [
> > > 					{name:"Effective Java",price:100},
> > > 					{name:"第一行代码 Java 版",price:40},
> > > 					{name:"数据结构与算法",price:25}
> > > 				]
> > > 			}
> > > 			
> > > 			const app = new Vue({
> > > 				el: "#app",
> > > 				// 从结果可以看出 data 数据是异步同时产生的，所以 dataAllNum 无法执行运算结果返回 NaN
> > > 				// NaN：返回的数据类型是 number，但是却又是不可运算的。
> > > 				data: testData,
> > > 				// 计算属性：通过属性的计算，返回一个新的属性。
> > > 				computed:{
> > > 					allNum:function(){
> > > 						return this.oneNum + this.twoNum + this.threeNum
> > > 					},
> > > 					// 进行较为复杂的逻辑运算，并返回结果。
> > > 					booksPrice:function(){
> > > 						let result = 0
> > > 						for(let book of this.books){
> > > 							result += book.price
> > > 						}
> > > 						return '总价:'+result;
> > > 					}
> > > 				},
> > > 				methods:{
> > > 					testAllNum:function(){
> > > 						return this.allNum + 5
> > > 					}
> > > 				}
> > > 			})
> > > 		</script>
> > > 	</body>
> > > </html>
> > > ```

### 8.1、computed 的 setter 和 getter 方法

> computed 计算属性时，默认只调用 getter  方法。
>
> > 若要调用 setter 方法，则当修改计算属性触发该方法。
> >
> > ```html
> > <!DOCTYPE html>
> > <html>
> > 	<head>
> > 		<meta charset="utf-8">
> > 		<title></title>
> > 	</head>
> > 	<body>
> > 		<div id="app">
> > 			<!-- 将 DOM 视图与数据属性 fullName 进行双向绑定 -->
> > 			fullName: <input type="text" v-model="fullName">
> > 			<h3 v-if="tag" style="color: red;">不符合规定的名字格式！</h3>
> > 			<h2>{{fullName}}</h2>
> > 		</div>
> > 		<script src="../js/vue.js"></script>
> > 		<script>
> > 			const testData = {
> > 				firstName: "Gzke",
> > 				lastName: "Mays",
> > 				tag:false
> > 			}
> > 			
> > 			const app = new Vue({
> > 				el: "#app",
> > 				data: testData,
> > 				computed:{
> > 					/** 默认调用 get 方法*/
> > 					// fullName: function(){
> > 					// 	return this.firstName + ' ' + this.lastName
> > 					// },
> > 					fullName: {
> > 						set: function(fullName){
> > 							let name = ''
> > 							if(fullName.indexOf(" ")>-1&& fullName != ''){
> > 								name = fullName.split(" ")
> > 								this.firstName = name[0]
> > 								this.lastName = name[1]
> > 								this.tag = false
> > 							}else{
> > 								this.tag = true
> > 								this.firstName = fullName
> > 								this.lastName = ''
> > 								console.log("不符合名字格式")
> > 							}
> > 						},
> > 						get: function(){
> > 							return this.firstName + ' ' + this.lastName
> > 						}
> > 					}
> > 				}
> > 			})
> > 		</script>
> > 	</body>
> > </html>
> > 
> > ```

## 九、事件监听：v-on

> 在`3.3`中，简单对 `v-on:click`进行使用，并对它的语法糖进行简单的讲解。所以此处就不再对 v-on 的使用进行示例演示。
>
> > 在 `v-on` 的使用中，我们可以利用它的修饰符来对事件进行控制。就如之前所讲的 `v-once` 一样起配置作用。 

### 9.1、v-on 修饰符之阻止冒泡事件

> 在开发中，如果遇到需要触发子级元素事件发生后，父级元素事件不被触发，则需要阻止冒泡。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<!-- .stop阻止事件冒泡 -->
			<div class="div0" @click="stopThis2">
			    点我事件2
			    <!-- stop阻止了事件冒泡，如果不加stop，父元素就会一起发生点击事件 -->
			    <button @click="stopThis">点我会触发事件冒泡</button>       
			    <button @click.stop="stopParent">点我不会触发事件冒泡</button>        
			</div>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const app = new Vue({
				el: "#app",
				methods:{
					stopParent(){
					    console.log("阻止事件冒泡 --- 阻止父类事件发生")
					    alert("阻止事件冒泡 --- 阻止父类事件发生")
					},
					stopThis(){
					    console.log("事件冒泡")
					    alert("事件冒泡")
					},
					stopThis2(){
					    console.log("事件2冒泡")
					    alert("事件2冒泡")
					}
				}
			})
		</script>
	</body>
</html>
```

### 9.2、v-on 修饰符之阻止默认行为

> 默认行为就是元素自带的行为事件，如：a 标签自带 href 链接跳转事件等……

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<!-- .prevent 阻止默认事件行为 -->
			<a href="http://www.baidu.com" >不阻止默认行为</a>
			<!-- 阻止原本事件的发生 -->
			<a href="http://www.baidu.com"@click.prevent="preventThis">阻止默认行为</a>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const app = new Vue({
				el: "#app",
				methods:{
					preventThis(){
							console.log("阻止默认事件的发生")
							alert("阻止了超链接事件的发生")
					}
				}
			})
		</script>
	</body>
</html>
```

### 9.3、v-on 修饰符之监听键

> 在控制行为发生时，我们可以对键盘的按键进行监听。如按回车键，按钮进行提交等……

```html
<!DOCTYPE html>
<html>
<header>
    <meta charset="UTF-8">
    <title>Learn1</title>
    <script src="js/vue.js"></script>
</header>

<body>
    <div id="app">
        <button @click="count = Math.random()">生成随机数</button>
        <p>自动生成随机数{{count}}</p>
        <input type="text" @keyup.space="submit" />
        <input type="text" @keyup.16="submit2" />
    </div>

    <script>
        var vm = new Vue({
            el:"#app",
            data:{
                count:0
            },
            methods: {
                submit(){
                    console.log("响应空格键");
                    alert("监听空格键");
                },
                submit2(){
                    console.log("响应Shift");
                    alert("监听Shift键"); 
                }
            }
        })

    </script>
</body>
</html>
```

## 十、根据条件来控制 DOM 元素：v-if

### 10.1、v-if 搭配 v-else-if 以及 v-else

> 与 `Thymeleaf` 类似，`Vue`也可以根据条件来控制 DOM 元素从而达到我们所需要的效果。
>
> > Thymeleaf：th:if=${ ... } 如果条件成立则该 DOM 元素显示。
> >
> > Vue：v-if=" ... " 如果条件成立 DOM 元素显示。
> >
> > > Vue 中，v-if 和 v-else 以及 v-else-if 若作用在 input 元素时，input 会进行复用，生成虚拟 DOM（放在内存当中）当条件成立时进行对比修改属性后再由虚拟 DOM 渲染到 HTML 页面中。
> > >
> > > ```html
> > > <!DOCTYPE html>
> > > <html>
> > > 	<head>
> > > 		<meta charset="utf-8">
> > > 		<title></title>
> > > 		<style type="text/css">
> > > 		</style>
> > > 	</head>
> > > 	<body>
> > > 		<!-- 简单的登陆信息提示案例 -->
> > > 		<div id="app">
> > > 			<h3 :style="h3Style" v-if="loginTag">{{tags}}</h3>
> > > 			<!-- v-if 是 v-else 的父亲，它们具有血缘关系。如果没 v-if 则 v-else 不能存在。 -->
> > > 			<h3 :style="h3Style" v-else>登陆系统</h3>
> > > 			<h3 :style="h3Style" v-if="testTag">注册系统</h3>
> > > 			
> > > 			UserName: <input type="text" v-model="username">
> > > 			PassWord: <input type="text" v-model="pwd">
> > > 			<button type="button" @click="judgeLogin()">登陆</button>
> > > 			<input type="text" v-model="mess" placeholder="e-mail" v-if="!toggleTag">
> > > 			<input type="text" v-model="mess" placeholder="telPhone" v-else>
> > > 			<button type="button" @click="toggleBtn()">切换</button>
> > > 		</div>
> > > 		<script src="../js/vue.js"></script>
> > > 		<script>
> > > 			const testData = {
> > > 				loginTag: false,
> > > 				testTag: false,
> > > 				toggleTag: false,
> > > 				username: "hwy",
> > > 				pwd: "12345",
> > > 				h3Style: {color: 'red'},
> > > 				tags: "",
> > > 				mess: ""
> > > 			}
> > > 			
> > > 			const app = new Vue({
> > > 				el: "#app",
> > > 				data: testData,
> > > 				methods:{
> > > 					judgeLogin(){
> > > 						if(this.username!='hwy'&&this.pwd=='12345'){
> > > 							this.tags = '用户名不正确！'
> > > 							this.loginTag = true
> > > 						}else if(this.pwd != "12345"&&this.username=='hwy'){
> > > 							this.tags = '密码不正确！'
> > > 							this.loginTag = true
> > > 						}else if(this.username!='hwy'&&this.pwd!='12345'){
> > > 							this.tags = '用户名以及密码都不正确！'
> > > 							this.loginTag = true
> > > 						}else{
> > > 							this.tags = '登陆成功！'
> > > 							this.loginTag = true
> > > 						}
> > > 						var timeOut = setTimeout(function() {
> > > 								app.loginTag = false
> > > 								this.tags = ''
> > > 							}, 1000);
> > > 					},
> > > 					toggleBtn(){
> > > 						this.toggleTag = !this.toggleTag
> > > 					}
> > > 				}
> > > 			})
> > > 		</script>
> > > 	</body>
> > > </html>
> > > ```

### 10.2、v-show 的使用

> v-show 的条件判断与 v-if 相同，只不过它们的处理方式不同。
>
> > v-if 是判断元素是否渲染，而 v-show 则是添加 display:none 的 style 来将 DOM 元素藏起来。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<div v-show="isShow">{{message}}</div>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData = {
				isShow: false,
				message: 'I Show'
			}
			const app = new Vue({
				el: "#app",
				data: testData
			})
		</script>
	</body>
</html>

```

## 十一、循环遍历

### 11.1、循环遍历数组对象

> 当对象是数组时，`(item,index) in movies` 获取的是数组的元素以及循环的次数。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<ul>
				<li v-for="(item,index) in movies">{{index+1}}——{{item}}</li>
			</ul>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData = {
				movies: [
					{
						id: 1,
						movie: "浪客剑心",
						price: 45.5
					},
					{
						id: 2,
						movie: "浪客剑心：传说",
						price: 50
					}
				]
			}
			
			
			const app = new Vue({
				el: "#app",
				data: testData
			})
		</script>
	</body>
</html>

```

### 11.2、循环遍历对象

> 当对象是一个属性的 map 时，`(value,key,index) in movies` 获取的是对象的元素的值和它的键，获取到的 index 是循环的次数。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<ul>
				<li v-for="(value , key , index) in data">{{index}} —— {{key}} —— {{value}}</li>
			</ul>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData = {
				data:{
					name: 'hwy',
					age: '23',
					gender: 'man',
					height: '174cm'
				}
			}
			const app = new Vue({
				el: "#app",
				data: testData
			})
		</script>
	</body>
</html>

```

### 11.3、 :key 的作用（高效率更新虚拟 DOM）

> key 和 id 类似，它在 Vue 中起一个唯一索引的作用。
>
> > 当我们需要查找某个元素的位置（循环遍历的元素）的位置时，Vue 则可以根据 key 来进行查找。
> >
> > > 如果没有 key 值，vue 在 v-for 循环进行插值操作时，会先把插值处的值进行修改，然后再往下依次更改。形成O(n) 的复杂度。
> > >
> > > 如果有 key 值，vue 在循环进行插值操作时，会根据 key 值前后进行直接插入（与 splice(end,start,new value) 方法相似），形成 O(1) 复杂度。
> > >
> > > > `:key` 值不可取 index 作为索引，因为插入数据后，index 与插入数据的实际 index 不相同，index 是循环遍历的次数，并非数组元素的索引。
> > > >
> > > > > Vue 中采取 Diff 算法来获取 key 值，并识别节点。之后，将插入值放入指定的位置。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<!-- 将 item 作为 key 绑定到 div 当中 -->
			<div v-for="(item,index) in dataArr" :key="item">{{item}}</div>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData = {
				dataArr: [1,2,3,4,5]
			}
			
			const app = new Vue({
				el: "#app",
				data: testData
			})
		</script>
	</body>
</html>
```

### 11.4、数组的响应式方法

> 数组数据进行响应式变化，就需要采取响应式的方法。
>
> - 响应式操作
>   - `push(...items)`：在数组最后的位置添加一个或多个元素。
>   - `unshift(...items)`：在数组头部位置添加一个或多个元素。
>   - `pop()`：删除数组最后一个元素。
>   - `shift()`：删除数组第一个元素。
>   - `splice(index,replaceNum,...items)`：对数组进行增、替、删的操作。
>   - `Vue.set(object,index,value)`：对数组某一个索引值进行替换。
>   - `sort()`：数组的升序。
>   - `reverse()`：数组的反转。
> - 非响应式操作
>   - `this.array[index] = value`：直接替换索引值的数据，不能做到响应式。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<ul>
				<li v-for="item in dataArr">{{item}}</li>
			</ul>
			<input type="text" v-model="numData">
			<button type="button" @click="addBtn()">增添数据</button>
			<button type="button" @click="powerBtn()">万能按钮</button>
			<button type="button" @click="delBtn()">删除数据</button>
			<button type="button" @click="sortBtn()">升序按钮</button>
			<button type="button" @click="reverseBtn()">反转按钮</button>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData = {
				dataArr: [3,2,5,6,1],
				numData: 0
			}
			
			const app = new Vue({
				el: "#app",
				data: testData,
				methods:{
					addBtn(){
						// 响应式在数组最后面添加一个或多个元素。
						this.dataArr.push(this.numData)
						// 响应式在数组最前面添加一个或多个元素。
						this.dataArr.unshift(parseInt(this.numData) + 1)
						// 该方法不是响应式的，它将在再修改的时才会发生变化。
						// this.dataArr[0] = this.numData
					},
					delBtn(){
						// 响应式删除数组中最后一个元素。
						this.dataArr.pop()
						// 响应式删除数组中第一个元素。
						this.dataArr.shift()
					},
					powerBtn(){
						// 响应式对数组进行元素的增添、删除、替换。
						// 第一个参数：index 开始的位置
						// 第二个参数：替换的或删除的数量
						// 第三个参数(...items)：增添或替换的值
						// 替换模式：若 items.length = replaceNum 则在 start 处进行替换模式。
						// 增\减替模式：若 items.length < replaceNum 则在 start 处进行减替。若 items.length > replaceNum 则在 start 处直接进行增替。
						let start = 2
						let replaceNum = 2
						// this.dataArr.splice(start,replaceNum,6,8,9,12);
						// 增添模式：若 replaceNum = 0 则在 start 处进行增添 items
						// this.dataArr.splice(start,replaceNum,3);
						
						// Vue 内部替换值的方法
						// Vue.set(对象，索引值，替换值)
						Vue.set(this.dataArr,0,5)
					},
					sortBtn(){
						// 升序
						this.dataArr.sort()
					},
					reverseBtn(){
						// 序列反转（倒过来）
						this.dataArr.reverse()
					}
				}
			})
		</script>
	</body>
</html>
```

## 十二、Script 高阶函数

> 在利用高阶函数我们可以避免使用 `for` 达到更加简洁高效的目的。
>
> > - 高阶函数
> >   - `array.filter(function(n){return Boolean.TRUE or Boolean.FALSE})`
> >     - 数组通过过滤器循环遍历，当过滤器返回 `true` 时，则返回 `n` 的值。否则不返回。
> >   - `array.map(function(n){return new_value})`
> >     - 通过 map 循环遍历，并将新的值`new_value`替换掉原来的值。
> >   - `array.reduce(function(preValue,currentValue,currentIndex,Array){return new_value},initValue)`
> >     - 通过  reduct 循环遍历进行汇总，每一次循环时返回的值：`preValue = new_value` 。
> >     - `currentValue` 则是返回遍历的元素。
> >     - `initValue` 则是定义初始化的值，也就是第一次循环的 `preValue`。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<script type="text/javascript">
			let dataArr = [58,12,43,50,100,30,120,105]
			
			/**
			 * 最基础的实现方式
			 */
			// 需求一：取出所有小于 100 的元素
			let numArr = []
			for (let i of dataArr){
				if (i<100){
					numArr.push(i)
				}
			}
			// 需求二：小于100的元素乘于2
			let biggerArr = []
			for (let i of numArr){
				i = i*2
				biggerArr.push(i)
			}
			// 需求三：将乘于2的数组元素全部相加求和
			let total = 0
			for (let i of biggerArr){
				total += i
			}
			console.log(total)
			/**
			 * 高阶函数实现需求
			 */
			// 需求一：取出所有小于 100 的元素
			let numArray = dataArr.filter(function(n){
				return n<100
			})
			// 需求二：小于100的元素乘于2
			let biggerArray = numArray.map(function(n){
				return n*2
			})
			// 需求三：将乘于2的数组元素全部相加求和
			let hTotal = biggerArray.reduce(function(preValue,n){
				console.log(preValue + "+" + n)
				return preValue + n
				},0)
			console.log(hTotal)
			
			// 高阶函数的总和以及简写
			let finTotal = dataArr.filter(function(n){return n<100}).map(function(n){return n*2}).reduce(function(pre,n){return pre+n})
			let finTotal2 = dataArr.filter(n => n<100).map(n => n*2).reduce((pre,n) => pre+n)
			console.log(finTotal)
			console.log(finTotal2)
		</script>
	</body>
</html>

```

## 十三、数据的双向绑定：v-model

> `Vue` 的数据双向绑定是实时进行的，不需要进行页面的刷新处理。
>
> > 当 DOM 元素绑定 Model 数据后，Vue 进行追踪并实现异步。

### 13.1、v-model 的实现原理

> `v-model` 的实现与 `:value` 触发 `@input `事件是一样的。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		
		<div id="app">
			<!-- :value 进行数据绑定 -->
			<!-- 需要绑定事件进行实时的数据传递 -->
			<input type="text" :value="message" @input="message = $event.target.value">
			{{message}}
			<hr />
			<!-- v-model 进行数据绑定 -->
			<!-- 不需要绑定事件即可实时进行数据的传递 -->
			<input type="text" v-model="message2">
			{{message2}}
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData = {
				message: "Hello Vue",
				message2: "Hello World"
			}
			
			const app = new Vue({
				el: "#app",
				data: testData,
				methods:{
					changeValue(event){
						this.message = event.target.value
					}
				}
			})
		</script>
	</body>
</html>

```

### 13.2、v-model 的数据绑定  —  单选按钮

> 使用 v-model 绑定单选按钮时，其数据若等于 radio 的 value 值，则为默认选取。
>
> > 单选的选取将根据绑定的数据是否等于其 value 值来决定。
> >
> > > 单选按钮组中的子元素，如果绑定的数据相同，则判断依据相同。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		
		<div id="app">
			<!-- 正常情况下实现单选按钮 -->
             <!-- name 为判断依据 -->
			<div class="mui-input-row mui-radio ">
				<label for="male">
					<input name="radio" id="male" value="男"  type="radio">男
				</label>
				<label for="female">
					<input name="radio" id="female" value="女"  type="radio" checked>女
				</label>
			</div>
			
			
			<!-- Vue 作用下实现单选按钮 -->
             <!-- 通过 v-model 的数据作为判断依据 -->
			<div class="mui-input-row mui-radio ">
				<label for="male2">
					<input v-model="sex" id="male2" value="男" type="radio">男
				</label>
				<label for="female2">
					<input v-model="sex" id="female2" value="女" type="radio">女
				</label>
			</div>
			<h2>你当前选择的性别是：{{sex}}</h2>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData = {
				sex: '男'
			}
			
			const app = new Vue({
				el: "#app",
				data:testData
			})
		</script>
	</body>
</html>
```

### 13.3、 v-model 的数据绑定 — 勾选框

> 勾选框有分单选和多选，单选的与  `radio` 相同。
>
> > 处理多选时则需要用一个数组进行数据装载。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<!-- 勾选框：1、单选；2、多选 -->
		
			<!-- 单选勾选框 -->
			<div class="mui-input-row mui-checkbox ">
				<h3>注册协议</h3>
				<label for="protocol">同意协议
					<input id="protocol" v-model="isProtocol" type="checkbox">同意协议
				</label>
				<h3>是否同意协议：{{isProtocol}}</h3>
			</div>
		
			<br>			
			<!-- 多选勾选框 -->
			<div class="mui-input-row mui-checkbox ">
				<h3>兴趣爱好</h3>
				<label for="basketBall">
					<input id="basketBall" type="checkbox" value="篮球" v-model="hobbeyArr"> 篮球
				</label>
				<label for="badminton">
					<input id="badminton"  type="checkbox" value="羽毛球" v-model="hobbeyArr"> 羽毛球
				</label>
				<label for="footBall">
					<input id="footBall"  type="checkbox" value="足球" v-model="hobbeyArr"> 足球
				</label>
				<label for="pingPong">
					<input id="pingPong" type="checkbox" value="乒乓球" v-model="hobbeyArr"> 乒乓球
				</label>
				<label for="volleyBall">
					<input id="volleyBall"  type="checkbox" value="排球" v-model="hobbeyArr"> 排球
				</label>
				<h3>兴趣爱好的数组：{{hobbeyArr}}</h3>
			</div>
			
			<br />
			<button type="button" :disabled="!isProtocol || hobbeyArr.length == 0">注册</button>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData = {
				isProtocol: false,
				hobbeyArr:[]
			}
			
			const app = new Vue({
				el: "#app",
				data: testData
			})
		</script>
	</body>
</html>
```

### 13.4、v-model 的数据绑定 — 选择框

> 其实现与勾选框类似，将数据绑定在 select 上。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		
		<div id="app">
			<!-- 单个选择框 -->
			<select name="selectBtn" v-model="selectData">
				<option value="Java">Java</option>
				<option value="Python">Python</option>
				<option value="Vue">Vue</option>
				<option value="Semantic">Semantic</option>
				<option value="Mybatis">Mybatis</option>
				<option value="Hibernate">Hibernate</option>
			</select>
			<h3>当前选择对象：{{selectData}}</h3>
			
			<!-- 多个选择框 -->
			<select name="selectBtn" v-model="selectArr" multiple="multiple">
				<option value="Java">Java</option>
				<option value="Python">Python</option>
				<option value="Vue">Vue</option>
				<option value="Semantic">Semantic</option>
				<option value="Mybatis">Mybatis</option>
				<option value="Hibernate">Hibernate</option>
			</select>
			<h3>当前选择对象：{{selectArr}}</h3>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData = {
				selectData: 'Java',
				selectArr: []
			}
			const app = new Vue({
				el: "#app",
				data:testData
			})
		</script>
	</body>
</html>

```

### 13.5、v-model 的值绑定

> 值绑定的作用是，为了避免将数据写死提高数据的灵活性。
>
> > 我们前端的显示是由服务器传递过来的数据进行动态显示的。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">	
			<!-- 多选勾选框 -->
			<div class="mui-input-row mui-checkbox ">
				<h3>兴趣爱好</h3>
				<label v-for="item in deafultArr" :for="item">
					<input type="checkbox" :id="item" :value="item" v-model="hobbeyArr"> {{item}}
				</label>
				<h3>兴趣爱好的数组：{{hobbeyArr}}</h3>
			</div>		
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData = {
				deafultArr: [ "Java", "Python", "Vue", "Semantic", "Mybatis", "Hibernate" ],
				hobbeyArr:[]
			}
			
			const app = new Vue({
				el: "#app",
				data:testData
			})
		</script>
	</body>
</html>

```

### 13.6、v-model 的修饰符

> 为了解决 v-model 绑定数据时的数据类型的需求或功能，Vue 提供了修饰符来改变 v-model 的规则。
>
> > - 修饰符
> >   - `v-model.lazy`：当对应的 DOM 元素失去焦点或点击回车时触发更新数据。
> >   - `v-model.number`：解决 input 的数据传递为 String 类型的问题。
> >   - `v-model.trim`：除去数据返回的空格。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<!-- 1.修饰符:lazy -->
			<!-- 为了减少 v-model 频繁地异步更新，我们可以通过 lazy 来实现当绑定数据的 DOM 元素失去焦点或按回车时再更新数据。 -->
			<input type="text" v-model.lazy="message">
			<h3>message 的值：{{message}}</h3>
			
			<!-- 2.修饰符：number -->
			<!-- input 传值默认为 string 类型，所以当我们需要获取 Integer 类型的数据时，我们需要 number 修饰符 -->
			<input type="text" v-model.number='age'>
			<h3>age 的值：{{age}} 以及它的类型：{{typeof age}}</h3>
			
			<!-- 3.修饰符：trim -->
			<input type="text" v-model.trim='longText'>
			<h3>longText 的值：{{longText}}</h3>
			
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData ={
				message:'',
				age:'',
				longText:''
			}
			const app = new Vue({
				el: "#app",
				data:testData
			})
		</script>
	</body>
</html>

```

## 十四、组件化

> 一个页面是由多个模块进行组成的，为了方便开发，以及方便将页面的模块进行区分开来。`Vue` 提供了组件化管理。
>
> > 组件化的作用有类似 `jsp` 中模块分割，可以将一个页面分割成多个模块。每个模块处理不同的数据逻辑，从而便于后期维护以及针对性开发。
> >
> > > 组件中可以再嵌套子组件，将功能进行再细分。

### 14.1 Vue 的组件使用的基本方式

> 组件的使用，继承 Vue 的 template 之后定义组件模板。
>
> > 使用 `Vue.compoent('compoent_name',tempalte)` 注册组件。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<!-- 第三步：使用组件 -->
			<my-cpn></my-cpn>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			// 第一步：定义模板
			const cpn = Vue.extend({
				template:`
					<div>
						<h2>Hello , World</h2>
						<p> Hi,Man </p>
					</div>
				`
			})
			// 第二步：注册组件
			Vue.component('my-cpn',cpn)
			const app = new Vue({
				el: "#app"
			})
		</script>
	</body>
</html>

```

### 14.2、定义全局组件和局部组件

> Vue 中我们的组件可以被唯一调用也可以被共享调用。
>
> > 因此我们组件分为全局组件和局部组件。
> >
> > > - 局部组件：定义在 Vue 实例当中。
> > > - 全局组件：定义在 Script 当中。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<!-- 第三步：使用组件 -->
			<h2>1.调用全局组件</h2>
			<my-cpn></my-cpn>
			<h2>2.调用 app 的组件</h2>
			<cpn2></cpn2>
		</div>
		
		<div id="app2">
			<!-- 全局组件可被调用 -->
			<h2>3.调用全局组件</h2>
			<my-cpn></my-cpn>
			<!-- app 的组件不能被调用在 app2 -->
			<h2>4.调用 app 的组件</h2>
			<cpn2></cpn2>
		</div>
		
		<script src="../js/vue.js"></script>
		<script>
			// 第一步：定义模板
			const cpn = Vue.extend({
				template:`
					<div>
						<h2>Hello , World</h2>
						<p> Hi,Man </p>
					</div>
				`
			})
			// 第二步：注册组件(全局组件，可以在多个 Vue 实例中被调用。)
			Vue.component('my-cpn',cpn)
			
			
			const app = new Vue({
				el: "#app",
				components:{
					// 第二步：注册组件(局部组件，只能在当前 Vue 实例中被调用。)
					cpn2: cpn
				}
			})
			
			const app2 = new Vue({
				el:"#app2"
				
			})
		</script>
	</body>
</html>

```

### 14.3、子组件和父组件的定义

> 已知，创建一个组件使用的是 `Vue.extends` 内的方法。
>
> > 注册组件的方法为：components，而 extends 也有该方法。所以在创建一个组件时，若定义了一个 components 则说明该组件拥有子组件。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">

			 <my_cpn></myCpn>
			 
			
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const childrenCpn = Vue.extend({
				template:`
					<div>
						<h3>大家好，我是子组件</h3>
					</div>
				`
			})
			
			
			const cpn = Vue.extend({
				template:`
					<div>
						<h2>大家好，我是组件！</h2>
						<childrenCPN></childrenCPN>
					</div>
				`,
				// 定义子组件：componets 
				components:{
					childrenCPN: childrenCpn
				}
			})
			
			Vue.component("my-cpn",cpn)
			
			
			const app = new Vue({
				el: "#app",
				components:{
					my_cpn: cpn
				}
			})
		</script>
	</body>
</html>

```

### 14.4、组件的语法糖

> Vue 中常用的方法一般都会带有语法糖，从而便于我们使用。
>
> > 组件的语法糖，省略了 extends 的定义，可以直接调用 component 方法注册一个组件的同时，定义该组件的模板。
> >
> > > 在 Vue 的实例中，可以在 componets 方法中直接以 key : {template : ...} 的形式直接定义组件。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		
		<div id="app">
			<cpn> </cpn>
			<cpn2></cpn2>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			// 基本用法
			const cpn = Vue.extend({
				template:`
					<div>
						<h2>大家好，我是组件！</h2>
						<childrenCPN></childrenCPN>
					</div>
				`
			})
			
			// 语法糖
			Vue.component('cpn',{
				template:`
					<div>
						<h2>大家好，我是组件！</h2>
						<childrenCPN></childrenCPN>
					</div>
				`
			})
			
			const app = new Vue({
				el: "#app",
				components:{
					cpn2:{
						template:`
						<div>
							<h2>大家好，我是组件2！</h2>
							<childrenCPN></childrenCPN>
						</div>
						
						`
					}
				}
			})
		</script>
	</body>
</html>

```

### 14.5、组件抽离的方式

> 即便使用语法糖为我们开发提供便利，但是随着组件的增加，我们代码会显得越来越多，会把 Vue 实例中变得更加臃肿。
>
> > 因此，我们可以将组件进行抽离，这样不仅方便我们对组件代码的查阅，也让代码显得更加规整。
> >
> > > - 抽离的两种方法
> > >   - 定义 `<script type="text/template" id='id_value'>` 的标签，根据 id 来获取模板。
> > >   - 定义 `<template id='id_value'>` 标签，根据 id 来获取模板。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		
		<div id="app">
			<cpn></cpn>
			<cpn2></cpn2>
		</div>
		
		<!-- 定义 template 类型的 script -->
		<script type="text/template" id="cpn">
			<div>
				<h2>大家好我是模板</h2>
			</div>
		</script>
		
		<!-- 定义 template 标签 -->
		<template id="cpn2">
			<h2>大家好我也是模板</h2>
		</template>
		
		<script src="../js/vue.js"></script>
		<script>
			const app = new Vue({
				el: "#app",
				components:{
					cpn:{
						template:"#cpn"
					},
					cpn2:{
						template:"#cpn2"
					}
				}
			})
		</script>	
	</body>
</html>

```

### 14.6、组件实现动态数据

> 在定义模板，往往会根据需求来改变渲染内容。但组件的内容不能直接从 Vue 实例的 data 中获取，即使能获取如果将数据都存放在 data 中，会把代码变得十分臃肿。
>
> > 子组件不能直接引用父组件的数据。
> >> 所以，组件的数据是有自己存放的地方的，在组件中也有个 data、methods 的属性用于定义组件的数据以及方法。
> > > 但 data 必须是一个函数。而且函数返回一个对象，对象内部保存数据。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<cpn></cpn>
		</div>
		
		
		<template id="hello">
			<div>
				<h2>{{message}}</h2>
			</div>
		</template>
		<script src="../js/vue.js"></script>
		<script>
			Vue.component('cpn',{
				template: "#hello",
				// data 是一个函数，他返回的对象是 Map 类型。
				data(){
					return{
						message: 'hello world'
					}
				}
			})			
			const app = new Vue({
				el: "#app"
			})
		</script>
	</body>
</html>

```

#### 14.6.1、组件的数据存储为什么是个函数？

> 组件是具有复用性的，也就是说该组件可以被重复调用。
>
> > 函数每次被调用或者创建时，它的内部栈空间 (内存地址) 是不相同的，所以它的内部数据不同的地方使用，都是以初始值开始。从而避免的数据共享的问题。
> >
> > > 如果组件的数据存储不是为函数的话，那么当组件被复用时，全部组件都共用一个数据 data ，这样就会导致数据的混乱。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<!-- 一个新的组件对象，counter 的初始值为 0 -->
			<cpn></cpn>
			<!-- 一个新的组件对象，counter 的初始值为 0 -->
			<cpn></cpn>
		</div>
		
		<template id="cpn">
			<div>
				<h3>当前计数：{{counter}}</h3>
				<button type="button" @click="increment">+</button>
				<button type="button" @click="decrement">-</button>
			</div>
		</template>
		<script src="../js/vue.js"></script>
		<script>
			Vue.component('cpn',{
				template: "#cpn",
				data(){
					return{
						counter:0
					}
				},
				methods: {
					increment(){
						this.counter++
					},
					decrement(){
						this.counter--
					}
				}
			})
			
			const app = new Vue({
				el: "#app"
			})
		</script>
	</body>
</html>

```

### 14.7、父子组件的通信

> 开发过程中，数据的获取往往是从大组件中得到的，然后再将数据分配给各个小组件。小组件则不需要发送请求。
>
> > 有时开发过程中也有小组件发送请求获取数据，再将数据传递给大组件。
> >
> > > 为了实现这个需求，首先就要解决父子组件之间通信的问题。
> > >
> > > > - 通信方法
> > > >   - 父传子`props(properties 的简写)`：定义一个获取父组件数据的属性，在<b>子组件</b>中使用 `v-bind:element = 'parentElement'` 进行绑定。
> > > >     - `propos` 定义属性常用的方法有两种
> > > >       - 一、数组定义法：`props:['element1','element2',....]` 数据类型由获取到的数据来决定。
> > > >       - 二、对象定义法：`props:{element1: dataType,element2: dataType ....}` 规定接收的数据类型。也可以定义数据的默认值：`props:{elements{type: dataType , default: value }}`。
> > > >         - dataType 包含： `string、number、boolean、array、object、date、function、symbol `
> > > >   - 子传父 `this.$emit('propertiesName',value)`
> > > >     - 子组件中实现事件触发绑定的方法，并传值。
> > > >     - 定义子组件在 `methods` 当中，为父组件获取子组件数据提供方法。在<b>子组件</b>中使用 `v-on:propertiesName = 'parentMethods'` 绑定方法。
> > > >
> > > > ![image-20200723224207535](..\img\image-20200723224207535.png)

#### 14.7.1、父传子

> `props` 对象内包含有很多方法，以下为部分
>
> > ![image-20200723232839050](..\img\image-20200723232839050.png)

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<!-- 属性 mess 获取父组件的 message，messArr 获取父组件的 messageArr -->
			<parent_cpn :mess='message'></parent_cpn>
		</div>
		
		<template id="cpn">
			<div>
				<h2>{{mess}}</h2>
				<ul>
					<li v-for="item in mess_arr">{{item}}</li>
				</ul>
			</div>
		</template>
		
		<script src="../js/vue.js"></script>
		<script>
			// 定义一个组件
			const cpn = Vue.extend({
				template: '#cpn',
				// 定义配置属性，用于被组件属性进行数据绑定。
				// 写法一：定义一个数组，来接收数据。它的类型是由获取到的数据来决定的。
				// props: ['mess','mess_arr'],
				
				// 写法二：定义一个对象，来定义数据类型。来规范获取数据的类型。
				props:{
					mess: {
						type: String,
						default: "我是 mess 的默认值",
						required: true
					},
					mess_arr: {
						type: Array,
						// Vue 的 2.5.17 以下会报错
						// default: []
						
						// 当数据类型是对象或者是数组时需要默认值为函数
						default(){return [1,2,3,4,5]}
					}
				}
				
			})
			
			// app 充当父组件
			const app = new Vue({
				el: "#app",
				data:{
					message:'我是父组件数据',
					messageArr: ['父1','父2','父3']
				},
				components:{
					parent_cpn: cpn
				}
			})
		</script>
	</body>
</html>

```

#### 14.7.2、子传父

> 子组件定义方法，在方法中编写  `$emit` 定义属性充当传数据的出口。
>
> > `this.$emit('出口名',携带的值)` 。使用 `v-on` 对方法实施绑定。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			ul li:hover{
				color: red;
			}
		</style>
	</head>
	<body>
		<div id="app">
			<!-- item_click 为 emit 的属性名，cpnClick 为父组件定义的方法。 -->
			<!-- 方法中获取到的值是 item_click 中封装好的 value 值。 -->
			<cpn @item_click="cpnClick"></cpn>
			<br>
			<h3>当前选中：{{currentName}}</h3>
		</div>
		
		<template id="cpn">
			<ul style="list-style: none;">
				<li v-for="item in navList" style="float: left; cursor: pointer;margin-left: 25px;" @click="itemClick(item.name)">{{item.name}}</li>
			</ul>
		</template>
		<script src="../js/vue.js"></script>
		<script>
			const cpn = {
				template: "#cpn",
				data(){
					return{
						navList: [
							{
								id: 1,
								name: '首页'
							},
							{
								id: 2,
								name: '标签'
							},
							{
								id: 3,
								name: '分类'
							}
						]
					}
				},
				methods:{
					itemClick(item_name){
						// 定义子组件要传出去的属性配置
						// this.$emit('propertiesName',value)
						this.$emit('item_click',item_name)
				}
				}
			}
			
			const app = new Vue({
				el: "#app",
				data:{
					currentName:''
				},
				methods:{
					cpnClick(name){
						this.currentName = name
					}
				},
				components:{
					cpn: cpn
				}
			})
		</script>
	</body>
</html>

```

##### 14.7.2.1、实现简单的计数器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
	当前计数：{{counter}}
	<cpn @increase='trend' @decrease='trend'></cpn>
</div>

<template id="cpn">
	<div>	
		<br>
		<button @click="increase"> + </button>
		<button @click="decrease"> - </button>
	</div>
</template>
<script src="../js/vue.js"></script>
<script>
	const cpn = {
		template: "#cpn",
		data(){
			return{
				counter:0
				}
			},
		methods:{
			increase(){		
				this.$emit('increase',this.counter++)
			},
			decrease(){
				this.$emit('decrease',this.counter--)
			}
		}
	}
	
	const app = new Vue({
		el: "#app",
		data:{counter:0},
		methods:{
			trend(counter){
				this.counter = counter
			}
		}
		components:{
			cpn: cpn
		}
	})
</script>
</body>
</html>
```

#### 14.7.3、父子组件的互通

> 父子互通的目的：父传子 — 子取值 — 修改返回给父值
>
> > 子组件不可以直接调用父组件的数据，那么通过 `props` 获取值后需要在子组件中建立 `data() 或 computed` 来对数据进行再获取。
> >
> > > 当子组件从方法中返回值时需要对数据类型进行转换<b>默认类型为 string </b>。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		
		<div id="app">
			<cpn :num1='num1' :num2='num2' @m_number1='getNumber1' @m_number2='getNumber2'/>
		</div>
		
		<template id="cpn">
			<div>
				<h3>props-num1 的值:{{num1}}</h3>
				<h3>num1 的值:{{number1}}</h3>
				<input type="text" v-model="number1">
				<input type="text" :value="number1" @input="mNumber1">
				<h3>props-num1 的值:{{num2}}</h3>
				<h3>num2 的值：{{number2}}</h3>
				<input type="text" v-model="number2">
				<input type="text" :value="number2" @input="mNumber2">
			</div>	
		</template>
		<script src="../js/vue.js"></script>
		<script>
			const cpn = {
				template: '#cpn',
				props:{
					num1:{
						type: Number
					},
					num2:{
						type: Number
					}
				},
				data(){
					return{
						number1: this.num1,
						number2: this.num2
					}
				},
				methods:{
					mNumber1(e){
						this.number1 = e.target.value
						this.$emit('m_number1',parseInt(this.number1))
					},
					mNumber2(e){
						this.number2 = e.target.value
						this.$emit('m_number2',parseInt(this.number2))
					}
				}

			}
			
			const testData = {
				num1: 1,
				num2: 2
			}
			const app = new Vue({
				el: '#app',
				data: testData,
				methods:{
					getNumber1(num1){
						this.num1 = num1
					},
					getNumber2(num2){
						this.num2 = num2
					}
				},
				components:{
					cpn: cpn
				}
			})
		</script>
	</body>
</html>
```

### 14.8、父子组件的访问方式

> 开发过程中，有时我们会需要父组件直接访问子组件，子组件直接访问父组件的需求。
>
> > 因此为了实现访问，Vue 为我们提供了相应的方法。
> >
> > - 父组件访问子组件：`$children 或 $refs`
> >   - `this.$children`：获取到的是数组对象，通过`this.$children[0]、this.$children[1]...` 方式指定子组件对象。 
> >   - `this.$refs`：获取到指定的 ref 对象，要创建 ref 对象，只需要在子组件中添加 ref 属性来指定 key 值即可。父组件可以通过 `this.$refs.key` 来获取对应的子组件。
> >
> > ```html
> > <!DOCTYPE html>
> > <html>
> > 	<head>
> > 		<meta charset="utf-8">
> > 		<title></title>
> > 	</head>
> > 	<body>
> > 		<div id="app">
> > 			<button @click="btnClick">父组件按钮</button>
> > 			<cpn ref='a'></cpn>
> > 			<cpn></cpn>
> > 		</div>
> > 		
> > 		<template id="cpn">
> > 			<button @click="btnClick">子组件按钮</button>
> > 		</template>
> > 		<script src="../js/vue.js"></script>
> > 		<script>
> > 			const cpn = {
> > 				template: '#cpn',
> > 				data(){return{message:'hello vue'}},
> > 				methods:{
> > 					btnClick(){
> > 						console.log(this.$parent)
> > 						console.log("showMessage")
> > 					}
> > 				}
> > 			}
> > 			
> > 			const app = new Vue({
> > 				el: "#app",
> > 				methods:{
> > 					btnClick(){
> > 						// 通过 $children 找到对应的组件
> > 						// console.log(this.$children)
> > 						// console.log(this.$children[0].message)
> > 						this.$children[0].btnClick()
> > 						// 通过 $refs.** 找到对应的 ref 组件
> > 						console.log(this.$refs.a)
> > 					}
> > 				},
> > 				components:{
> > 					cpn: cpn
> > 				}
> > 			})
> > 		</script>
> > 	</body>
> > </html>
> > 
> > ```
> >
> > 
> >
> > - 子组件访问父组件：`$parent` 和 `$root` （使用较少）
> >   - `this.$parent`：访问与其绑定的父组件，可以调用父组件的属性以及数据。
> >   - `this.$root`：访问其注册的 Vue 实例，同理可以调用其属性以及数据。
> >
> > ```html
> > <!DOCTYPE html>
> > <html>
> > 	<head>
> > 		<meta charset="utf-8">
> > 		<title></title>
> > 	</head>
> > 	<body>
> > 		<div id="app">
> > 			<button @click="btnClick">根组件按钮</button>
> > 			<cpn></cpn>
> > 		</div>
> > 		
> > 		<template id="cpn">
> > 			<button @click="btnClick">子组件按钮</button>
> > 		</template>
> > 		
> > 		<template id="p_cpn">
> > 			<div>
> > 				<button >父组件按钮</button>
> > 				<cpn></cpn>
> > 			</div>
> > 		</template>
> > 		<script src="../js/vue.js"></script>
> > 		<script>
> > 			const cpn = {
> > 				template: '#cpn',
> > 				data(){return{message:'hello vue'}},
> > 				methods:{
> > 					btnClick(){
> > 						// this.$parent 访问父组件
> > 						console.log(this.$parent)
> > 						console.log(this.$parent.btnClick())
> > 						
> > 						// this.$root 访问根组件（Vue 实例）
> > 						console.log(this.$root)
> > 					}
> > 				}
> > 			}
> > 			
> > 			const p_cpn = {
> > 				template: '#p_cpn',
> > 				methods:{
> > 					btnClick(){
> > 						
> > 						console.log("I'm parent methods")
> > 					}
> > 				},
> > 				components:{
> > 					cpn: cpn
> > 				}
> > 				
> > 			}
> > 			
> > 			const app = new Vue({
> > 				el: "#app",
> > 				methods:{
> > 					btnClick(){
> > 						console.log("I'm root methods")
> > 					}
> > 				},
> > 				components:{
> > 					cpn: p_cpn,
> > 				}
> > 			})
> > 		</script>
> > 	</body>
> > </html>
> > 
> > ```
> >
> > 

## 十五、插槽 slot

> 插槽的作用扩展业务，在现实中，比如一个电脑它有许多插槽，可以插入鼠标 USB、电脑 USB 从而扩展电脑功能。Vue 中也是如此，用于扩展组件功能。
>
> > 在开发中，我们组件可能被多次调用，因此如果调用同一个组件，实现显示不同的内容。那就要对该组件进行扩展。让不同的内容预留存放空间。
> >
> > > 一般作用于具有共性的页面，同则保留不同则更换插槽。

### 15.1、slot 的基本使用

> 在 Vue 中，要定义插槽才可以在组件中添加元素。
>
> > 插槽在 `<slot></slot>` 标签内添加标签则相当于给插槽制定默认值。当组件中没有插入元素时调用。
> >
> > > 当组件中插入其他元素，则覆盖插槽中的元素。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<cpn>
				<button>我覆盖了</button>
				<button>我一起覆盖！</button>
			</cpn>
			<cpn></cpn>
		</div>
		
		<template id="cpn">
			<div>
				<h2>Hello Vue</h2>
				<slot><button>我是插槽的默认按钮</button></slot>
			</div>
		</template>
		<script src="../js/vue.js"></script>
		<script>
			const cpn = {
				template: '#cpn'
			}
			
			const app = new Vue({
				el: "#app",
				components:{
					cpn: cpn
				}
			})
		</script>
	</body>
</html>
```

### 15.2、slot 具名插槽

> 为插槽制定名字，相当于制定你电脑的 USB  接口一样，我们可以根据需求修改对应的 slot 插槽。
>
> > 具体制定名字：在组件中为插槽添加 `name` 属性。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<!-- 初始的组件 -->
			<cpn></cpn>
			<cpn>
				<span slot="left"><button><—</button></span>
				<span slot="center"><input type="text" placeholder="搜索"></span>
				<span slot="right">导航栏</span>
			</cpn>
		</div>
		
		<template id="cpn">
			<div>
				<slot name="left">左</slot>
				<slot name="center">中</slot>
				<slot name="right">右</slot>
			</div>
		</template>
		<script src="../js/vue.js"></script>
		<script>
			const cpn = {
				template: '#cpn'
			}
			
			const app = new Vue({
				el: "#app",
				components:{
					cpn: cpn
				}
			})
		</script>
	</body>
</html>
```

### 15.3、编译作用域

> 在使用组件时，我们已经了解父传子，子传父的数据互通方法。那么在直接调用数据时，我们就要想`Vue` 调用的是父组件的数据，还是子组件的数据？
>
> > 答案是它会调用父组件的数据，对于父组件来说，子组件在它的作用里只不过起一个 HTML 标签的作用，和普通的 `div` 没有什么区别，因此在直接调用数据做判断时，调用的是其所在的父组件数据。
> >
> > > 数据同名不会覆盖，因为组件相当于一个新的单例。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<!-- 调用 Vue 根组件的 isShow -->
			<cpn v-show="isShow"></cpn>
		</div>
		
		<template id="cpn">
			<div>
				<h2>我是子组件</h2>
			</div>
		</template>

		<template id="p_cpn">
			<div>
				<h2>我是父组件</h2>
				<!-- 调用 p_cpn 中的 isShow -->
				<cpn v-show="isShow"></cpn>
			</div>
		</template>
		
		<script src="../js/vue.js"></script>
		<script>
			const cpn = {
				template: '#cpn',
				data(){return {
					isShow: false
				}}
			}
			
			const p_cpn = {
				template: '#p_cpn',
				data(){return {
					isShow: true
				}},
				components:{
					cpn: cpn
				}
			}
			
			const app = new Vue({
				el: "#app",
				data:{
					isShow: true
				},
				components:{
					cpn: p_cpn
				}
			})
		</script>
	</body>
</html>

```

### 15.4、作用域插槽

> 作用域插槽的作用：父组件替换插槽的标签，但插槽的内容由子组件提供。简单的来说，子组件定义了初始数据在插槽中进行渲染，但是父组件不希望这些数据显示，那么父组件就要提供数据替换子组件的初始数据。
>
> > 因为在 `15.3` 中我们对作用域的了解是，一个组件中，它的子组件直接调用的数据是由组件本身提供的。<b>因此子组件的渲染模式，也可以由父组件来决定。</b>
> >
> > > 具体方法：在子组件插槽中通过`v-bind:*` 定义属性名，而后父组件中定义`<template>`  模板，在模板中通过引用 `slot-scope="*"，新版本可用 v-slot="*" 代替` 指定作用域（指定插槽）。最后在父组件中通过 `*.*` 即可引用对应的数据。
> > >
> > > > 通过 `v-bind:*` 自定义属性名绑定数据时，不能使用 Vue 已制定的别名。如：`class` 、`style` 等……

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<cpn></cpn>
		</div>
		
		<!-- 子组件 -->
		<template id="cpn">
			<div>
				<slot v-bind:efg="messArr">
					<ul>
						<li v-for="item in messArr">{{item}}</li>
					</ul>
				</slot>
			</div>
		</template>

		<!-- 父组件 -->
		<template id="p_cpn">
			<div>
				<h2>我是父组件</h2>
				<cpn></cpn>
				<cpn>
					<template v-slot="abc">
						<div>
							<span>{{abc.efg.join(" - ")}}</span>
						</div>
					</template>
				</cpn>
			</div>
		</template>
		
		<script src="../js/vue.js"></script>
		<script>
			const cpn = {
				template: '#cpn',
				data(){return {
					messArr:["子组件数据1","子组件数据2","子组件数据3"]
				}}
			}
			
			const p_cpn = {
				template: '#p_cpn',
				components:{
					cpn: cpn
				}
			}
			
			const app = new Vue({
				el: "#app",
				components:{
					cpn: p_cpn
				}
			})
		</script>
	</body>
</html>

```



## 十六、模块化开发

> 在以往开发中，由于全局变量的命名丰富，当导入多个 js 文件时，若有属性名相同，则取最后导入的 js 文件的数据。这样就会导致另一个 js 文件的数据失效。
>
> > 为了避免这类问题的发生，有以下解决方案。
> >
> > - 匿名函数：将数据使用 `function(){}` \ `;(function(){})` 概括起来，规定作用域。但是该方法却不能代码不可互用。
> >
> > - 模块化：编写 js 文件时，定义全局方法并返回该 js 可能被调用的参数列表。但模块的名称不应相同。
> >
> >   - ```javascript
> >     ;# moduleA.js
> >     var moduleA = (function(){
> >     	let obj = {}
> >     	flag = true
> >     	if(flag){
> >     		console.log("I'm module A , the flag is true")
> >     	}
> >     	
> >     	obj.flag = flag
> >     	return obj
> >     })()
> >     
> >     ;# moduleB.js
> >     var moduleB = (function(){
> >     	let obj = {}
> >     	let flag = false
> >     	if(!flag){
> >     		console.log("I'm module B , the flag is false")
> >     	}
> >     	
> >     	obj.flag = flag
> >     	return obj
> >     })()
> >     
> >     ;# moduleC.js
> >     var moduleC = (function(){
> >     	if(moduleA.flag){
> >     		console.log("I'm module C , I use module A flag")
> >     	}
> >     	if(moduleB.flag){
> >     		console.log("I'm module C , I use module B flag")
> >     	}
> >     })()
> >     ```
> >
> >   - ```html
> >     <!DOCTYPE html>
> >     <html>
> >     	<head>
> >     		<meta charset="utf-8">
> >     		<title></title>
> >     	</head>
> >     	<body>
> >     		<!-- 测试模块调用 -->
> >     		<script src="../js/moduleA.js"></script>
> >     		<script src="../js/moduleB.js"></script>
> >     		<script src="../js/moduleC.js"></script>
> >     	</body>
> >     </html>
> >     
> >     ```
> >
> >     - <b>模块化规范已经有许多，目前常见的有：CommonJS、AMD、CMD、ES6。（ES5 时没有模块化概念）</b>
> >
> > > 模块化的核心是导入和导出，导入调用，导出输出。
> > >
> > > - CommonJS(Node)
> > >
> > >   - `module.exports = {a:'a' , b:'b'}` 自动将里面的数据进行导出。
> > >   - `var {a,b} = require('*.js') 或 var test = require('*.js')  test.a = a  `
> > >
> > > - ES6
> > >
> > >   - `<script src="../js/moduleA2.js" type="module"></script>` 指定导入的 JS 为模块。
> > >
> > >   - 模块的导出 （可导出属性和函数）
> > >   ```javascript
> > >     // A
> > >     let flag = true
> > >     let message = "I'm module A , the flag is true"
> > >     if(flag){
> > >     	console.log(message)
> > >     }
> > >     // 使用 export 在调用该 js 模块时导出数据
> > >     export{
> > >     	flag ,message
> > >     }
> > >     
> > >     // B
> > >     let flag = true
> > >     let message = "I'm module B , the flag is true"
> > >     if(flag){
> > >     	console.log(message)
> > >     }
> > >     //  使用 export 在调用该 js 模块时导出封装好的数据
> > >     let m =  {flag:flag,message:message}
> > >     export{
> > >     	m
> > >     }
> > >     //  直接导出
> > >     export var num = 1
> > >   
> > >     // 默认导出，不规定命名。deafult 默认唯一。
> > >     export default a  
> > >     
> > >   ```
> > >   
> > >   - 模块的导入（可导入数据和函数）
> > >   
> > >   ```javascript
> > >   import {flag}	from "./moduleA2.js"
> > >   import {m} from "./moduleB2.js"
> > >   import {num} from "./moduleB2.js"
> > >   import a from "./moduleB2.js"
> > >   
> > >   import * as am from "./moduleA2.js"
> > >   
> > >   if(flag){
> > >   	console.log("I'm module C , I use module A flag")
> > >   	console.log("I'm module C , I use module A flag"+ am.message)
> > >   }
> > >   if(m.flag){
> > >   	console.log("I'm module C , I use module B flag " + num +  a)
> > >   
> > >   }
> > >   
> > >   ```
> > >
> > > 





























































































## 实操示例

### 一、点击切换样式

> 运用三目元算符对 index 进行判断并实现 class 的添加。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<style type="text/css">
		ul{
			list-style: none;
		}
		li{
			float: left;
			margin-left: 50px;
		}
		li:hover{
			cursor: pointer;
		}
		.active{
			color: red;
		}
	</style>
	<body>
		<div id="app">
			<ul>
				<li 
				 v-for="(movie,index) in movies"
				 @click="changeClass(index)"
				 :class="{active: isItem == index}">
						{{index+1}} — {{movie}}
				</li>
			</ul>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData = {
				movies:["海王","肖生克的救赎","那一年我们错过的女孩","飞驰人生"],
				isItem: 0
			}
			
			const app = new Vue({
				el: "#app",
				data:testData,
				methods:{
					changeClass:function(itemIndex){
						this.isItem = itemIndex
					}
				}
			})
		</script>
	</body>
</html>

```

### 二、简易的购物车

> 根据数组的数据拷贝进行逻辑判断实现添加与删除。
>
> > 数组的数据拷贝是公用的。
> >
> > > 使用过滤器处理价格。
> > >
> > > > 使用 `:disabled` 处理超出购买数量的时的事件。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<h3>商品列表</h3>
			<table border="5" cellspacing="8" cellpadding="8">
				<thead>
					<tr>
						<th>ID</th>
						<th>书本名</th>
						<th>价格</th>
						<th>库存</th>
						<th>操作</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="(item,index) in products">
						<td>{{index}}</td>
						<td>{{item.productName}}</td>
						<td>{{item.price | showPrice}}</td>
						<td>{{item.inventory}}</td>
						<td><button type="button" @click="bougth(index)">添加进购物车</button></td>
					</tr>
				</tbody>
			</table>
			
			
			<h3>购物车</h3>
			<table border="5" cellspacing="8" cellpadding="8">
				<thead>
					<tr>
						<th>ID</th>
						<th>书本名</th>
						<th>价格</th>
						<th>购买数量</th>
						<th>操作</th>
					</tr>
				</thead>
				<tbody>
					<tr v-if="shoppingCart.length != 0" v-for="(item,index) in shoppingCart">
						<td>{{index}}</td>
						<td>{{item.productName}}</td>
						<td>{{item.price | showPrice}}</td>
						<td>{{item.beBought}}</td>
						<td>减少<input type="text" v-model="item.delNum">个 <button @click="delProduct(index)" :disabled="item.delNum > item.beBought">确认减少</button></td>
					</tr>
				</tbody>
			</table>
			<h2>总价：{{total | showPrice}}</h2>
		</div>
		<script src="../js/vue.js"></script>
		<script>
			const testData = {
				products:[
					{
						id: 1,
						productName: "数据结构与算法 Java 语言的描述",
						price: 10.50,
						inventory: 2,
						beBought: 0,
						delNum:0
					},
					{
						id: 2,
						productName: "第一行代码 Java 版",
						price: 20.99,
						inventory: 3,
						beBought: 0,
						delNum:0
					},
					{
						id: 3,
						productName: "Effective Java",
						price: 30.50,
						inventory: 5,
						beBought: 0,
						delNum:0
					}
				],
				shoppingCart:[]
			}
			
			
			const app = new Vue({
				el: "#app",
				data: testData,
				methods:{
					bougth(index){	
						if(this.shoppingCart.length == 0){
							// 数据是公用的。
							this.shoppingCart.push(this.products[index])
							this.products[index].beBought++
							this.products[index].inventory--
						}else if(this.products[index].inventory == 0){
							alert("该书籍已经没有库存!")
						}else{
							if(this.shoppingCart.indexOf(this.products[index]) == -1){
								this.shoppingCart.push(this.products[index])
								this.products[index].beBought++
								this.products[index].inventory--
							}else{
								this.products[index].beBought++
								this.products[index].inventory--
							}
						}
					},
					delProduct(index){
						if(this.shoppingCart[index].delNum < this.shoppingCart[index].beBought){
							this.shoppingCart[index].beBought = parseInt(this.shoppingCart[index].beBought) -  parseInt(this.shoppingCart[index].delNum)
							this.shoppingCart[index].inventory = parseInt(this.shoppingCart[index].inventory) + parseInt(this.shoppingCart[index].delNum)
						}else{
							if(this.shoppingCart.indexOf(this.shoppingCart[index]) > -1){
								let cartIndex = this.shoppingCart.indexOf(this.products[index])
								this.shoppingCart.splice(cartIndex,1)
								this.products[index].beBought = parseInt(this.products[index].beBought) -  parseInt(this.products[index].delNum)
								this.products[index].inventory = parseInt(this.products[index].inventory) + parseInt(this.products[index].delNum)
							}
						}
					}
				},
				computed:{
					total(){
						let total = 0
						if(this.shoppingCart.length != 0){
							return this.shoppingCart.reduce((pre,book) => pre + book.beBought * book.price,0)
						}
						return total
						
					}
				},
				filters:{
					// 使用过滤器处理数据。
					showPrice(price){
						return price.toFixed(2) + "元"
					}
				}
			})
		</script>
	</body>
</html>

```

