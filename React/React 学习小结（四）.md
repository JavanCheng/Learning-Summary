*所有的 Tips 均为本人 OneNote 中的笔记，复制过来就是图片，懒得转文本了*

# 第五章：React 路由
## 一、相关理解
### 1.1 SPA的理解
1.	单页Web应用（single page web application，SPA）
2.	整个应用只有**一个完整的页面**
3.	点击页面中的链接**不会刷新**页面，只会做页面的**局部更新**
4.	数据都需要通过ajax请求获取, 并在前端异步展现
5.	**单页面，多组件**

### 1.2 路由的理解
1. 什么是路由？
	（1）一个路由就是一个映射关系 (key:value)
	（2）key 为路径, value 可能是 function 或 component
2. 路由分类
	（1）后端路由
	a. 理解： value是function, 用来处理客户端提交的请求
	b.	注册路由： `router.get(path, function(req, res))`
	c. 工作过程：当node接收到一个请求时, 根据请求路径找到匹配的路由, 调用路由中的函数来处理请求, 返回响应数据
	（2）前端路由
	a. 浏览器端路由，**value是component，用于展示页面内容**
	b. 注册路由: `<Route path="/test" component={Test}>`
	c. 工作过程：当浏览器的 path 变为 /test 时, 当前路由组件就会变为 Test 组件
>Tips：路由
>1. 点击导航栏，路径改变，路由监测到路径改变，将对应组件展现出来
>2. 路由监测路径改变的是浏览器历史记录，浏览器的历史记录是一个栈的结构
>3. 除了 push 入栈，还可以通过 replace 替换历史记录
>4. 前端路由通过 BOM 身上的 history 实现
### 1.3 react-router-dom 的理解
1. react 的一个插件库
2. 专门用来实现一个 SPA应用
3. 基于 react 的项目基本都会用到此库
## 二、react-router-dom 相关 API
### 2.1 内置组件
1. `<BrowserRouter>`
2.	`<HashRouter>`
3.	`<Route>`
4.	`<Redirect>`
5.	`<Link>`
6.	`<NavLink>`
7.	`<Switch>`
### 2.2 其他
1. history 对象
2. match 对象
3. withRouter 函数
## 三、基本路由使用
### 3.1 准备
1. 下载 react-router-dom: npm install --save react-router-dom
2.	引入 bootstrap.css: `<link rel="stylesheet" href="/css/bootstrap.css">`

### 3.2 举个例子

```javascript
import React, { Component } from 'react'
import {Link,Route} from 'react-router-dom'
import Home from './components/Home'
import About from './components/About'

export default class App extends Component {
	render() {
		return (
			<div>
				<div className="row">
					<div className="col-xs-offset-2 col-xs-8">
						<div className="page-header"><h2>React Router Demo</h2></div>
					</div>
				</div>
				<div className="row">
					<div className="col-xs-2 col-xs-offset-2">
						<div className="list-group">

							{/* 原生html中，靠<a>跳转不同的页面 */}
							{/* <a className="list-group-item" href="./about.html">About</a>
							<a className="list-group-item active" href="./home.html">Home</a> */}
							{/* 在React中靠路由链接实现切换组件--编写路由链接 */}
							<Link className="list-group-item" to="/about">About</Link>
							<Link className="list-group-item" to="/home">Home</Link>
						</div>
					</div>
					<div className="col-xs-6">
						<div className="panel">
							<div className="panel-body">
								{/* 注册路由 */}
								<Route path="/about" component={About}/>
								<Route path="/home" component={Home}/>
							</div>
						</div>
					</div>
				</div>
			</div>
		)
	}
}

```
### 3.3 总结
1. 注意写法，要在 App 组件外用 BrowserRouter 包裹起来，形成一个路由器，在同一个路由器内发送请求
2. 要明确好界面中的导航区、展示区
3. 导航区的a标签改为Link标签（Link 标签被编译转换成 a 标签，并加入了一些路由代码）
`<Link to="/xxxxx">Demo</Link>`
4. 展示区写Route标签进行路径的匹配
`<Route path='/xxxx' component={Demo}/>`
>`<App>` 的最外侧包裹了一个 `<BrowserRouter>` 或 `<HashRouter>`
![在这里插入图片描述](https://img-blog.csdnimg.cn/21aed53201c043688829546712c2dfa6.png)
## 四、路由组件与一般组件
1. 写法不同：
	一般组件：`<Demo/>`
	路由组件：`<Route path="/demo" component={Demo}/>`
2. 存放位置不同：
	一般组件：components
	路由组件：pages
3. 接收到的 props 不同：
一般组件：写组件标签时传递了什么，就能收到什么
路由组件：接收到三个固定的属性
	1. history:
		go: ƒ go(n)
		goBack: ƒ goBack()
		goForward: ƒ goForward()
		push: ƒ push(path, state)
		replace: ƒ replace(path, state)
	2. location:
		pathname: "/about"
		search: ""
		state: undefined
	3. match:
		params: {}
		path: "/about"
		url: "/about"

## 五、NavLink 的使用
### 5.1 使用

```javascript
<NavLink activeClassName="atguigu" className="list-group-item" to="/about">About</NavLink>
<NavLink activeClassName="atguigu" className="list-group-item" to="/home">Home</NavLink>
```
### 5.2 封装
封装代码如下：

```javascript
import React, { Component } from 'react'
import {NavLink} from 'react-router-dom'

export default class MyNavLink extends Component {
	render() {
		// console.log(this.props);
		return (
			<NavLink activeClassName="atguigu" className="list-group-item" {...this.props}/>
		)
	}
}
```
调用代码如下：

```javascript
<MyNavLink to="/about">About</MyNavLink>
<MyNavLink to="/home">Home</MyNavLink>
```
打印 this.props，结果如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/c79fb62345b94222b84609f94c747284.png)
"About" 为标签体内容，在 props 中的 key 为 children
所以封装时可用 ...this.props 将标签值全传递过去
### 5.3 总结
1. NavLink可以实现路由链接的高亮，通过 activeClassName 指定样式名
2. 标签体内容是一个特殊的属性标签
3. 通过 this.props.children 可以获取标签体内容
4. 改成NavLink会自动激活bootstrap的样式，选中区高亮，可能会和之后自己设置的样式产生冲突
5. 可以在自定义的样式后加上 `!important`，将样式的层级调到最高

## 六、Switch 的使用

```javascript
{/* 注册路由 */}
<Switch>
	<Route path="/about" component={About}/>
	<Route path="/home" component={Home}/>
	<Route path="/home" component={Test}/>
</Switch>
```
1. 通常情况下，path 和 component 是一一对应的关系
2. 注册一个以上路由时用 Switch在外层包下，可以在进行路由匹配时，匹配到对应的路由后即停止匹配，提高效率

## 七、解决多级路径刷新页面样式丢失的问题
1. 问题出现的原因
	>localhost:3000是react脚手架内置的服务器
	靠webpack中的devServer开启
	而devServer的底层就是express
	public是内置服务器的根路径
	如果请求一个不存在的路径
	那么会返回根路径下的index.html
	当路由路径是多级的形式
	如/ccnu/home
	在刷新时bootstrap样式会丢失

2. 解决问题
	>1. public/index.html 中 引入样式时不写 ./ 写 / （常用）
	>2. public/index.html 中 引入样式时不写 ./ 写 %PUBLIC_URL% （常用）
	>3. 使用HashRouter

## 八、路由的严格匹配与模糊匹配
模糊匹配：

```javascript
// home a b 可与 home 匹配
<MyNavLink to="/home/a/b">Home</MyNavLink>
<Route path="/home" component={Home}/>

// a home b 不可与 home 匹配
<MyNavLink to="/a/home/b">Home</MyNavLink>
<Route path="/home" component={Home}/>
```
严格匹配：

```javascript
<MyNavLink to="/home/a/b">Home</MyNavLink>
<Route exact path="/home" component={Home}/>
```
总结：
1. 默认使用的是模糊匹配（简单记：【输入的路径】必须包含要【匹配的路径】，且顺序要一致）
2. 开启严格匹配：`<Route exact={true} path="/about" component={About}/>`
3. 严格匹配不要随便开启，需要再开，有些时候开启会导致无法继续匹配二级路由
## 九、Redirect的使用
1. 一般写在所有路由注册的最下方，当所有路由都无法匹配时，跳转到 Redirect（重定向） 指定的路由
2. 具体编码：
```javascript
<Switch>
	<Route path="/about" component={About}/>
	<Route path="/home" component={Home}/>
	<Redirect to="/about"/>
</Switch>
```
## 十、嵌套路由的使用
1. 注册子路由时要写上父路由的path值
2. 路由的匹配是按照注册路由的顺序进行的
## 十一、向路由组件传递参数数据
>分三步走
>1. 传递参数
>2. 声明接收参数
>3. 接收参数
1. params参数
路由链接(携带参数)：`<Link to='/demo/test/tom/18'}>详情</Link>`
注册路由(声明接收)：`<Route path="/demo/test/:name/:age" component={Test}/>`
接收参数：`this.props.match.params`
2. search参数
路由链接(携带参数)：`<Link to='/demo/test?name=tom&age=18'}>详情</Link>`
注册路由(无需声明，正常注册即可)：`<Route path="/demo/test" component={Test}/>`
接收参数：`this.props.location.search`
备注：获取到的search是urlencoded编码字符串，需要借助querystring解析
3. state参数
路由链接(携带参数)：`<Link to={{pathname:'/demo/test',state:{name:'tom',age:18}}}>详情</Link>`
注册路由(无需声明，正常注册即可)：`<Route path="/demo/test" component={Test}/>`
接收参数：`this.props.location.state`
备注：刷新也可以保留住参数
## 十二、push 与 replace
>从路由原理上看，默认切换路由的操作是一个 压栈的push操作， 对历史记录进行操作
所以如果我们依次点击消息1、消息2、 消息3，再点击回退，就会依次回到消息2，消息1的界面
在路由链接中开启replace模式，则对历史记录的操作会由压栈变为替换，不会回退到消息2、消息1

使用：

```javascript
<MyNavLink replace to="/about">About</MyNavLink>
```
## 十三、编程式路由导航
借助this.prosp.history对象上的API对操作路由跳转、前进、后退
（1）this.prosp.history.push()
（2）this.prosp.history.replace()
（3）this.prosp.history.goBack()
（4）this.prosp.history.goForward()
（5）this.prosp.history.go()
>push和replace方法，在传递路由查看方式的时候，可以被调用来改变查看方式，具体的path还是分3种写法
(params, search, state) ，要一一对应
goBack和goForward方法是前进和后退的意思
go方法要携带参数如1, 2，-1,分别表示前进一步，前进两步，后退一-步
## 十四、withRouter 的使用
1. 功能
接收一般组件,将其加上路由组件所特有的三个props参数，即history, match, locaton
2. 写法

```javascript
import React, { Component } from 'react'
import {withRouter} from 'react-router-dom'

class Header extends Component {
	back = ()=>{
		this.props.history.goBack()
	}
	forward = ()=>{
		this.props.history.goForward()
	}
	go = ()=>{
		this.props.history.go(-2)
	}
	render() {
		console.log('Header组件收到的props是',this.props);
		return (
			<div className="page-header">
				<h2>React Router Demo</h2>
				<button onClick={this.back}>回退</button>&nbsp;
				<button onClick={this.forward}>前进</button>&nbsp;
				<button onClick={this.go}>go</button>
			</div>
		)
	}
}
//withRouter可以加工一般组件，让在这里插入代码片一般组件具备路由组件所特有的API
//withRouter的返回值是一个新组件
export default withRouter(Header)
```
## 十五、BrowserRouter与HashRouter的区别
1. 底层原理不一样：
（1）BrowserRouter使用的是H5的history API，不兼容IE9及以下版本
（2）HashRouter使用的是URL的哈希值
2. path表现形式不一样
（1）BrowserRouter的路径中没有 #,例如：`localhost:3000/demo/test`
（2）HashRouter的路径包含 #,例如：`localhost:3000/#/demo/test`
3. 刷新后对路由state参数的影响
（1）BrowserRouter没有任何影响，因为state保存在history对象中。
（2）HashRouter（没有借用 history 这个 API，所以没有地方保存 state 参数）刷新后会导致路由state参数的丢失！！！
4. 备注：HashRouter可以用于解决一些路径错误相关的问题。

# 第六章：React UI组件库
## 一、流行的开源 React UI组件库
### 1.1 material-ui (国外)
1. 官网：[material-ui](http://www.material-ui.com/#/)
2. github：[material-ui](https://github.com/callemall/material-ui)
### 1.2 ant-design (国内蚂蚁金服)
1. 官网：[antd](https://ant.design/index-cn)
2. github：[antd](https://github.com/ant-design/ant-design/)
## 二、antd的按需引入+自定主题
1. 安装依赖：`yarn add react-app-rewired customize-cra babel-plugin-import less less-loader`
2. 修改package.json
```
....
	"scripts": {
		 	"start": "react-app-rewired start",
			"build": "react-app-rewired build",
			"test": "react-app-rewired test",
			"eject": "react-scripts eject"
	},
....
```	
3. 根目录下创建 config-overrides.js

```
//配置具体的修改规则
const { override, fixBabelImports,addLessLoader} = require('customize-cra');
	module.exports = override(
		fixBabelImports('import', {
				libraryName: 'antd',
			    libraryDirectory: 'es',
				style: true,
		}),
		addLessLoader({
			lessOptions:{
				javascriptEnabled: true,
				modifyVars: { '@primary-color': 'green' },
			}
		}),
	);
```
4. 备注：不用在组件里亲自引入样式了，即：`import 'antd/dist/antd.css'`应该删掉
>Tips
>1. 引入后在官网查询代码使用即可
>2. 类似的还有 elementUI（主要用于 Vue）、vant（主要用于移动端）
>3. 适用于做一些成熟的后台管理系统（样式较为固定）
>4. 按需引入：用到哪个样式就引入哪个样式，不需要引入 antd.css，具体配置看官方文档
>5. 自定义主题：按需更改默认样式

## React 系列小结：
[React 学习小结（一）](https://blog.csdn.net/APythonC/article/details/121954352)
[React 学习小结（二）](https://blog.csdn.net/APythonC/article/details/121960138)
[React 学习小结（三）](https://blog.csdn.net/APythonC/article/details/121978288)
[React 学习小结（四）](https://blog.csdn.net/APythonC/article/details/121984342)
[React 系列小结（五）](https://blog.csdn.net/APythonC/article/details/122015076)
[React 系列小结（六）](https://blog.csdn.net/APythonC/article/details/122017918)
