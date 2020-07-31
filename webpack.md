# webpack

> webpack 支持模块化开发，会自动对模块中所用的模板（CommanJS、ES6、AMD、CMD）进行打包成浏览器识别的代码的模板。
>
> > 前端打包有许多工具，其中 grunt/gulp 比较常见，但是 grunt/glup 更加强调的是前端流程自动化，模块化不是核心。而 webpack 则是以模块化为核心，强调模块化开发管理，文件压缩、预处理等……则是附带功能。
> >
> > > webpack 模块化打包，为了正常运行，它必须依赖 node 环境。node 环境是为了正常的执行很多代码，必须其中包含各种依赖的包。为了方便包的下载，可以调用 npm 工具。

## 一、webpack 打包的起步

> 使用 npm 安装 webpack：`npm install webpack -g`
>
> > 在 4.0 版本后，webpack 的打包需要对环境的 mode 进行配置。在项目中创建 webpack.config.js，在其中导入依赖
> >
> > ```javascript
> > module.exports = {
> > 	mode : 'development'   // development为开发者环境，production为生产环境变量 ，还有一个为none
> > }
> > ```
> >
> > > 制定好模块后，关联使用。

```javascript
# mathUtils.js
function sum(num1,num2){
	return num1+num2
}
function mul(num1,num2){
	return num1*num2
}
// export {
// 	sum,mul
// }

// 使用 module.exports 导出方法
module.exports = {
	sum,mul
}

# testData.js
export let num1 = 5
export let num2 = 6

# main.js
// 使用 require 导入方法 -- CommandJS
const{sum,mul} = require('./mathUtils.js')
// ES6 导入方法
import * as data from './testData.js'
console.log(sum(data.num1,data.num2))
console.log(mul(data.num1,data.num2))
```

> > > > 而后，在项目中打开命令行，使用 `webpack ./src/main.js` 将 main.js 进行打包。4.0 后直接输入就将打包成在 dist 目录下，生成 main.js。

## 二、webpack 的配置

> webpack 的使用大都是先在配置文件中，配置 webpack 的打包规则。因此在使用前我们一般会将该项目进行初始化处理。`npm init` 为我们生成 `package.json` 文件。
>
> > 在 `package.json` 文件中，可以配置出入口，以及当前环境（4.0版本后必须配置）
> >
> > ```javascript
> > // 全局的 webpack 自带的 js
> > const path = require('path')
> > 
> > module.exports = {
> > 	entry: './src/main.js',
> > 	output: {
> > 		// 使用绝对路径 __dirname 获取当前路径，绝对路劲的拼接：resolve(当前路径,目录)
> > 		path: path.resolve(__dirname,'dist'),
> > 		filename: 'bundle.js'
> > 	},
> > 	mode : 'development'   // development为开发者环境，production为生产环境变量 ，还有一个为none
> > }
> > ```
> >
> > > 配置完成后，直接运行 `webpack` 即可进行打包。一般开发中采用 `npm run build` 执行。因为 `webpack` 当在多配置文件时，它需要额外进行指定。
> > >
> > > > 项目的 webpack 版本不一定与开发者的 webpack 版本一直，因此在安装项目版本时，可以安装局部 `webpack` ： `cnpm install webpack@<version> --save-dev`
> > > >
> > > > > 调用本地 webpack 时，需要进入到本地的 webpack 目录。

## 三、webpack 的 loader

> 在处理打包 css、json 等其他资源时，webpack 需要通过 loader 来实现扩充。[具体官网](https://www.webpackjs.com/loaders/css-loader/) 通过需求来安装不同的依赖。

### 3.1、处理打包 css 文件

- css-loader — 只负责将 css 文件进行加载，不进行解析和生效。

  - 安装 `css-loader` ：`cnpm install --save-dev css-loader@2.0.2` 

  - 在 `webpack.config.js` 中配置使用规则：

    - ```javascript
        module: {
          rules: [
            {
              test: /\.css$/,
              use: [ 'css-loader' ]
            }
          ]
        }
      ```

  - 安装时要注意版本的高低，如果 loader 的版本高于 webpack 则打包失败。案例版本为 3.6.0

- style-loader — 负责将样式添加到 DOM 元素中使其生效。

  - 安装 `style-loader` ： `cnpm install --save-dev style-loader@0.23.1`

  - 在 `webpack.config.js` 中配置使用规则：

    - ```javascript
      // 全局的 webpack 自带的 js
      const path = require('path')
      
      module.exports = {
      	entry: './src/js/main.js',
      	output: {
      		// 使用绝对路径
      		path: path.resolve(__dirname,'dist'),
      		filename: 'bundle.js'
      	},
        module: {
          rules: [
            {
      				// 正则表达式匹配 css 文件
              test: /\.css$/,
              use: [ 'style-loader','css-loader' ]
            }
          ]
        }
      }
      ```

    - <b>webpack 调用 use 的 loader 时是从右向左进行配置的。因此 style-loader 必须放在 css-loader 前。起到先获取 css 再解析 style 的目的。</b>

- less-loader — 解析 less 文件（实际上可以转化成 css 文件后直接编译）

  - 安装 `less-loader`：`cnpm install --save-dev less-loader@4.1.0 less`

  - 在 `webpack.config.js` 中配置使用规则：

    - ```javascript
      // 全局的 webpack 自带的 js
      const path = require('path')
      
      module.exports = {
      	entry: './src/js/main.js',
      	output: {
      		// 使用绝对路径
      		path: path.resolve(__dirname,'dist'),
      		filename: 'bundle.js'
      	},
        module: {
          rules: [
            {
      				// 正则表达式匹配 css 文件
              test: /\.css$/,
              use: [ 'style-loader','css-loader' ]
            },
      			{
      				test: /\.less$/,
      				use: [{
      						loader: "style-loader" // creates style nodes from JS strings
      				}, {
      						loader: "css-loader" // translates CSS into CommonJS
      				}, {
      						loader: "less-loader" // compiles Less to CSS
      				}]
      			}
          ]
        }
      }
      ```

### 3.2、处理图片资源

> 当 css 文件中有背景图片等资源需要打包时则需要额外的 loader 来扩充我们的打包业务。
>
> > - 小文件资源 `url-loader`：安装步骤相同，当图片资源在 limit 范围内时，`url-loader`  可以直接调用打包。
> >
> >   - 转换成的资源是按照 Base64 编码将图片字节转换成字符串。
> >
> >   - ```javascript
> >     // 全局的 webpack 自带的 js
> >     const path = require('path')
> >     
> >     module.exports = {
> >     	entry: './src/js/main.js',
> >     	output: {
> >     		// 使用绝对路径
> >     		path: path.resolve(__dirname,'dist'),
> >     		filename: 'bundle.js'
> >     	},
> >       module: {
> >         rules: [
> >           {
> >     				// 正则表达式匹配 css 文件
> >             test: /\.css$/,
> >             use: [ 'style-loader','css-loader' ]
> >           },
> >     			{
> >     				test: /\.less$/,
> >     				use: [{
> >     						loader: "style-loader" // creates style nodes from JS strings
> >     				}, {
> >     						loader: "css-loader" // translates CSS into CommonJS
> >     				}, {
> >     						loader: "less-loader" // compiles Less to CSS
> >     				}]
> >     			},
> >     			{
> >     				test: /\.(png|jpg|gif)$/,
> >     				use: [
> >     					{
> >     						loader: 'url-loader',
> >     						options: {
> >     							limit: 13000
> >     						}
> >     					}
> >     				]
> >     			}
> >         ]
> >       }
> >     }
> >     ```
> >
> >   - 
> >
> > - 大文件资源 `file-loader` ： 安装步骤相同，当图片资源在 limit 范围外时，则需要加载 `file-loader` 模块。
> >
> >   - 转换成的资源是将图片进行打包一并放在 dist 目录中。
> >
> >   - 但是默认加载是按照开发的时候来的，因此我们需要设置打包的资源路径。
> >
> >   - `file-loader` 的文件生成是通过 Hash 值来随机生成的，若要制定生成规则，则需要配置 `name`
> >
> >   - ```javascript
> >     // 全局的 webpack 自带的 js
> >     const path = require('path')
> >     
> >     module.exports = {
> >     	entry: './src/js/main.js',
> >     	output: {
> >     		// 使用绝对路径
> >     		path: path.resolve(__dirname,'dist'),
> >     		filename: 'bundle.js',
> >     		publicPath: 'dist/'
> >     	},
> >       module: {
> >         rules: [
> >           {
> >     				// 正则表达式匹配 css 文件
> >             test: /\.css$/,
> >             use: [ 'style-loader','css-loader' ]
> >           },
> >             {
> >                 test: /\.less$/,
> >                 use: [{
> >                     loader: "style-loader" // creates style nodes from JS strings
> >                 }, {
> >                     loader: "css-loader" // translates CSS into CommonJS
> >                 }, {
> >                     loader: "less-loader" // compiles Less to CSS
> >                 }]
> >             },
> >             {
> >                 test: /\.(png|jpg|gif)$/,
> >                 use: [
> >                     {
> >                         loader: 'url-loader',
> >                         options: {
> >                             limit: 13000,
> >                             // [name] 获取图片名字的变量 name，[hash:8] 截取哈希值的前8位，[ext] 获取后缀。
> >                             name: 'img/[name].[hash:8].[ext]'
> >                         },
> >     
> >     
> >                     }
> >                 ]
> >             }
> >         ]
> >       }
> >     }
> >     ```

### 3.3、ES6 转 ES5

> 对于不支持 ES6 的浏览器，我们需要进行兼容时。就要考虑将 ES6 转换成 ES5。但 `webpack` 默认是不会自带转换功能的，因此我们需要 `babel` 的 loader 进行转换。
>
> > 安装命令（webpack 为 3.6.0）： `cnpm instal --save-dev babel-loader@7.1.5 babel-core@6.26.3 babel-preset-es2015@6.24.1` 安装 env：``cnpm install babel-loader@8.0.0-beta.0 @babel/core @babel/preset-env``
> >
> > 配置属性
> >
> > ```javascript
> > {
> >     test: /\.js$/,
> >         // 排除在外的资源，这些资源不转换 ES5。
> >         exclude: /(node_modules|bower_components)/,
> >             use: {
> >                 loader: 'babel-loader',
> >                     options: {
> >                         presets: ['@babel/preset-env']
> >                     }
> >             }
> > }
> > 
> > ```
> >
> > > 打包后的 js 文件是不包含 `const \ let` 等 ES6 定义属性。

## 四、webpack 配置 Vue

> 在利用 webpack 打包时，我们可以实现将 Vue 的依赖注入我们的 js 当中一同打包，这样就不需要再额外对 Vue 进行导入。
>
> > - 具体实现
> >
> >   - 项目环境安装 Vue 依赖：`cnpm install vue --save` vue 是在开发以及发布时都要使用，因此不仅限于 -dev。
> >
> >   - 安装成功后，在入口 js 文件对 Vue 进行导入：`import Vue from 'vue'` webpack 会自动的在本地仓库 `node_module` 内查安装的 vue 并进行导入。
> >
> >   - Vue 构建版本的时候有 `runtime-only` 和 `runtime-compiler`
> >
> >     - `runtime-only`：代码中不能带有任何 template，初始的 `app` 也归属于 template。
> >
> >     - `runtime-compiler`：代码中允许带有 template。
> >
> >     - <b>在默认中，Vue 的运用是 runtime-only 模式。因此我们需要对它进行模式的修改。</b>
> >
> >       - ```javascript
> >         resolve: {
> >             // alias：别名
> >             // git commit -m '注释'
> >             // git c ''
> >             // git config alias ''
> >             alias: {
> >                 // vue.esm 包含 compiler
> >                 'vue$': 'vue/dist/vue.esm.js'
> >             }
> >         }
> >         ```
> >

### 4.1、创建 Vue 时 template 和 el 的关系

> 采用 Vue 开发，我们一般采取 <b>SPA(Simple page web application 单页面网络应用)</b>在处理多个页面时，我们可以使用 vue-router 前端路由来进行处理。
>
> > 在主程序入口中，我们在 Vue 实例编写 template 后用 webpack 打包之后，template 中的 HTML 内容会替换掉 el 对应的 DOM 元素。从而渲染到页面。

```javascript
const{sum,mul} = require('./mathUtils.js')
// import * as math from './mathUtils.js'
import * as data from './testData.js'

console.log(sum(data.num1,data.num2))
console.log(mul(data.num1,data.num2))

import Vue from 'vue'

const app = new Vue({
	el: "#app",
	data: {
		message: "Hello webpack"
	},
	template:`
		<div>
			<h2>{{message}}
			<button>我是 template 的按钮</button>
		</div>
	`
})
```

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<div id="app">
		</div>
		
		<script src="./dist/bundle.js"></script>
		<!-- <script src="./src/main.js" type="module"></script> -->
	</body>
</html>

```

### 4.2、template 插入组件

> 在上述例子中，我们可以看到 template 可以自动替换渲染 `<div id="app"></div>` 。因为 Vue 实例中的同级属性它们是异步的，因此我们可以注册组件，将组件内容插入 template 中。
>
> > 因此为了可以便于开发管理，我们可以将组件编写进不同的 JS 文件当中。然后在主 JS 文件中导入，并在 Vue 实例注册组件后插入 template 当中。	
> >
> > ![image-20200730090810087](..\img\image-20200730090810087.png)

```javascript	
# headmess.js
export default {
		data(){return{
			message: "Hello webpack"
		}},
		methods:{
			consoleMessage(){
				console.log("hello webpack")
			}
		},
		template:`
			<div>
				<h2>{{message}}
				<button @click="consoleMessage">我是 template 的按钮</button>
			</div>
			`
}

# main.js
import Vue from 'vue'
import headmess from './vue/headmess.js'
new Vue({
	el:"#app",
	template:'<headmess/>',
    // 可以不定义别名
	components:{
		headmess
	}
})
```

### 4.3、创建 vue 文件

> 在上述例子中，我们将代码进行分离从而便于管理。但是还不足够，因为编写 template 没有语法的快捷键，在编写多业务时效率非常低，因此 Vue 为我们提供一个专门的分离式文件`vue`。
>
> > 为了 `*.vue` 文件能够成功被打包，我们需要配置 `vue-loader` 和 `vue-template-compiler`：`cnpm install vue-loader vue-template-compiler --save-dev`
> >
> > > 安装后在 `webpack.config.js` 中配置
> > >
> > > ```javascript
> > > const { VueLoaderPlugin } = require('vue-loader')
> > > module: {
> > >     {
> > >         test: /\.vue$/,
> > >         use: ['vue-loader']
> > >     }
> > > }
> > > plugins: [
> > >     new VueLoaderPlugin()
> > > ]
> > > ```
> > >
> > > 

```vue
<template>
	<div>
		<h2>{{message}}
		<button @click="consoleMessage">我是 template 的按钮</button>
	</div>
</template>

<!-- 与平常的 javascript 相同，支持 import 导外包。 -->
<script>
	export default {
		data(){return{
			message: "Hello webpack"
		}},
		methods:{
			consoleMessage(){
				console.log("hello webpack")
			}
		}
	}
</script>

<!-- 定义样式 -->
<style scoped="scoped">
	body{
		background-color: red;
	}
</style>

```

### 4.4、webpack 的 plugin 配置

> plugin 相当于插件，用于扩展当前应用的功能。许多应用都会创建插件，webpack 也不例外。
>
> > 在此简要对 webpack 的 plugin 进行说明。

#### 4.4.1、版权设置

> 为打包的主 JS 文件声明版权

```javascript
// 全局的 webpack 自带的 js
const webpack = require('webpack')
module.exports = {
	entry: './src/js/main.js',
	output: {
		// 使用绝对路径
		path: path.resolve(__dirname,'dist'),
		filename: 'bundle.js',
		publicPath: 'dist/'
	},
	plugins: [
		new webpack.BannerPlugin('版权终归 mays 所有')
	]
}
```

#### 4.4.2、打包 html 

> HtmlWebPackPlugin 自动生成 index.html 文件，将打包 js 文件，自动通过 script 标签插入 body 中。
>
> > 安装：`cnpm install html-webpack-plugin --save-dev`

```javascript
const { VueLoaderPlugin } = require('vue-loader')
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const uglifyjs = require('uglifyjs-webpack-plugin')

plugins: [
    new VueLoaderPlugin(),
    new webpack.BannerPlugin('版权终归 mays 所有'),
    new HtmlWebpackPlugin({
        // 设置模板
        template: 'index.html'
    })
]
```

#### 4.4.3、压缩 js

> UglifyjsWebPackPlugin ，将 js 进行压缩，CLI2 指定版本号为 1.1.1。
>
> > 安装：`cnpm install uglifyjs-webpack-plugin@1.1.1 --save-dev`

```javascript
const { VueLoaderPlugin } = require('vue-loader')
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const uglifyjs = require('uglifyjs-webpack-plugin')

plugins: [
    new VueLoaderPlugin(),
    new webpack.BannerPlugin('版权终归 mays 所有'),
    new HtmlWebpackPlugin({
        // 设置模板
        template: 'index.html'
    }),
    new uglifyjs()
]
```

## 五、webpack 搭建本地服务器

> 本地服务器和 tomcat 相似，在往后开发中，我们的项目可能会遇到进行前后端分离时其前端占一个服务器，后端占一个服务器的情况，从而前端获取后端数据进行跨域访问。
>
> > 安装（对应 webpack3.2.0版本）：`cnpm install --save-dev webpack-dev-sever@2.9.1`
> >
> > - devServer 属于 options
> >   - contentBase：指定一个文件夹提供本地服务，默认是根目录。此处指定 `./dist`
> >   - port：端口号
> >   - inline：页面实时刷新
> >   - historyApiFallback：在 SPA 页面中，依赖 HTML5 的 history 模式
> >
> > > 配置完成后
> > >
> > > 运行方法一：在终端执行 `./node_modules/.bin/webpack-dev-server`
> > >
> > > 运行方法二：在`package.json` 中配置 scripts：
> > >
> > > - ```javascript
> > >     "name": "mwebpack",
> > >     "version": "1.0.0",
> > >     "description": "",
> > >     "main": "index.js",
> > >     "host": "localhost",
> > >     "scripts": {
> > >       "build": "webpack",
> > >     		"dev": "webpack-dev-server --open"
> > >     },
> > >   ```
> > >
> > > - 终端中运行`npm run build` 在 GitBash 运行报错，cmd 中运行成功。

## 六、webpack 配置文件的分离

> 在 Java 开发中，我们知道运行环境可以根据 appliction-dev、appliction-prod 对不同的环境进行运行。
>
> > webpack 也可以对不同的环境进行分离。为了让配置文件可以进行相互合并扩充，因此我们需要 `webpack-merge` 的辅助。
> >
> > > 安装：`cnpm install webpack-merge`
> > >
> > > > 在自定义 base.config.js 文件之后，我们不再需要 webpack.config.js，但是 webpack 默认需要 webpack.config.js 因此，我们需要对默认进行修改。
> > > >
> > > > ```json
> > > > "scripts": {
> > > >     // 添加 --config 对运行环境进行配置
> > > >     "build": "webpack --config ./build/prod.config.js",
> > > >     "dev": "webpack-dev-server --open --config ./build/prod.config.js"
> > > > },
> > > > ```
> > > >
> > > > 

```javascript
// 配置生产环境的扩充 base
const uglifyjs = require('uglifyjs-webpack-plugin')
const {merge} = require('webpack-merge')
const baseConfig = require('./base.config.js')

module.exports = merge(baseConfig,{
		plugins: [
			new uglifyjs()
		],
	}
)
```

