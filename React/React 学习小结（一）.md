*所有的 Tips 均为本人 OneNote 中的笔记，复制过来就是图片，懒得转文本了*

# 第一章：React入门
## 一、React简介
### 1.1 概念
React 是一个将数据渲染为 HTML 视图的开源 JavaScript 库
由 Facebook（现 meta） 开源
### 1.2 工作流程
1. 发送请求获取数据
2. 处理数据（过滤、整理格式等）
3. **操作 DOM 呈现页面**
### 1.3 为什么要学 React（React 有什么优点 / 原生 JS 的缺点）
1. 原生 JavaScript 操作 DOM 繁琐、效率低（**React 有成熟的 DOM-API 操作 UI**）
2. 使用 JavaScript 直接操作 DOM，浏览器会进行大量的重绘重排（**React 通过优秀的 Diffing 算法优化 DOM 操作**）
3. 原生 JavaScript 没有**组件化编码**方案，代码复用率低

### 1.4 React 的特点
1. 采用**组件化**模式、**声明式编码**，提高开发效率及组件复用率
2. 在 **React Native** 中可以使用 React 语法进行 **移动端开发**
3. 使用 **虚拟DOM** + 优秀的 **Diffing算法**，尽量减少与真实 DOM 的交互，提高效率
### 1.5 举个例子
<img src="https://img-blog.csdnimg.cn/c4a5c2c597a240af975c29ff1d0e0feb.png" width="50%">

情景：包含 100 个用户的列表
要求：新增一个用户
原生 JS 做法：新生成 101 个 DOM，去覆盖掉原来 100 个 DOM，效率低
React 做法：在虚拟 DOM 中与之前的 DOM 进行比较，最终生成新增的 DOM，效率高（生成增量）
## 二、React 的基本使用
### 2.1 相关 JS 库
1. react.js ：React核心库
2. react-dom.js ：提供操作 DOM 的 React 扩展库
3. babel.min.js ：解析 JSX 语法代码转化为 JS 代码的库，还会将 ES6 转化为 ES5
### 2.2 Hello React 案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>hello_react</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>

	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel" > /* 此处一定要写babel */
		//1.创建虚拟DOM
		const VDOM = <h1>Hello,React</h1> /* 此处一定不要写引号，因为不是字符串 */
		//2.渲染虚拟DOM到页面
		ReactDOM.render(VDOM,document.getElementById('test'))
	</script>
</body>
</html>
```
### 2.3 创建虚拟 DOM 的两种方式
1. 使用 JSX 创建
```javascript
//1.使用 JSX 创建虚拟DOM
const VDOM = (  /* 此处一定不要写引号，因为不是字符串 */
	<h1 id="title">
		<span>Hello,React</span>
	</h1>
)
```
2. 使用 JS 创建（不推荐）

```javascript
//2.使用 JS 创建虚拟DOM
const VDOM = React.createElement('h1'{id:'title'},React.createElement('span',{},'Hello,React'))

```
### 2.4 虚拟 DOM 与 真实 DOM
1. 本质是 Object 类型的对象（一般对象）
2. 虚拟 DOM 比较“轻”，真实 DOM 比较“重”，因为虚拟 DOM 是 React 内部在用，无需真实 DOM 上那么多的属性
3. 虚拟 DOM 最终会被 React 转化为真实 DOM，呈现在页面上
4. 我们编码时基本只需要操作 React 的虚拟 DOM 相关数据，React 会转换为真实 DOM 变化，从而更新界面
>Tips:
>babel会开启严格模式，this指向undefined

## 三、React JSX
### 3.1 JSX 简介
全称：JavaScript XML
定义：类似于 XML 的 JS 扩展语法
本质：React.createElement(component, props,...,children) 方法的语法糖（语法糖可以理解为就是一种封装好的简便写法）
作用：用来简化创建虚拟 DOM
需注意的点：
1.不是字符串，也不是 HTML/XML 标签
2.最终产生一个 JS 对象
3.标签名任意：HTML 标签或其他标签
4.标签属性任意：HTML 标签属性或其他
5.浏览器不能直接运行 JSX 代码，需要 babel 转译为纯 JS 代码才能运行
### 3.2 JSX 语法规则
1. 定义虚拟DOM时，不要写引号
2. 标签中混入JS表达式时要用{}
3. 样式的类名指定不要用class，要用**className**
4. 内联样式，要用style={{key:value}}的形式去写
5. 只有一个根标签
6. 标签必须闭合
7. 标签首字母
(1) 若小写字母开头，则将该标签转为html中同名元素，若html中无该标签对应的同名元素，则报错
(2) 若大写字母开头，react就去渲染对应的组件，若组件没有定义，则报错
### 3.3 渲染虚拟 DOM
1. 语法

```javascript
ReactDOM.render(virtualDOM, containerDOM)
```
2. 作用：将虚拟 DOM 元素渲染到页面的真实容器 DOM 中显示
3. 参数说明
（1）virtualDOM：纯 JS 或 JSX 创建的虚拟 DOM 对象
（2）containerDOM：用来包含虚拟 DOM 的真实 DOM 元素对象（一般是一个 div）
### 3.4 举个例子
```javascript
const myId = 'CCNU'
const myData = 'Hello, React'
//1.创建虚拟DOM
const VDOM = (
	<div>
		<h2 className="title" id={myId.toLowerCase()}>
			<span style={{color:'white',fontSize:'29px'}}>{myData.toLowerCase()}</span>
		</h2>
		<h2 className="title" id={myId.toUpperCase()}>
			<span style={{color:'white',fontSize:'29px'}}>{myData.toLowerCase()}</span>
		</h2>
		<input type="text"/>
	</div>
)
//2.渲染虚拟DOM到页面
ReactDOM.render(VDOM,document.getElementById('test'))
```
## 四、模块与组件、模块化与组件化
### 4.1 模块
1. 理解：向外提供特定功能的 JS 程序, 一般就是一个 JS 文件
2. 为什么要拆成模块：随着业务逻辑增加，代码越来越多且复杂
3. 作用：复用 JS , 简化 JS 的编写, 提高 JS 运行效率
### 4.2 组件
1.	理解：用来实现局部功能效果的代码和资源的集合(html/css/js/image等等)
2.	为什么要用组件： 一个界面的功能更复杂
3.	作用：复用编码, 简化项目编码, 提高运行效率
> Tips： 模块化编码 与 组件化编码 的区别
>![在这里插入图片描述](https://img-blog.csdnimg.cn/2087d95214aa416e9fcc4c5fffdd5453.png)
### 4.3 模块化
当应用的 JS 都以模块来编写的, 这个应用就是一个模块化的应用
### 4.4 组件化
当应用是以多组件的方式实现, 这个应用就是一个组件化的应用
组件化也是工程化的一部分
### 4.5 举个例子
![在这里插入图片描述](https://img-blog.csdnimg.cn/7b058b7fc0c04e6e88fd16ca348b2649.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQVB5dGhvbkM=,size_20,color_FFFFFF,t_70,g_se,x_16)
包含三个组件：Header、LeftMenu、AllCate
如 Header 组件，可能是包含了 html/css/js/img/video/font...，这些所有实现头部功能的元素组合起来形成 Header 组件

## React 系列小结：
[React 学习小结（一）](https://blog.csdn.net/APythonC/article/details/121954352)
[React 学习小结（二）](https://blog.csdn.net/APythonC/article/details/121960138)
[React 学习小结（三）](https://blog.csdn.net/APythonC/article/details/121978288)
[React 学习小结（四）](https://blog.csdn.net/APythonC/article/details/121984342)
[React 系列小结（五）](https://blog.csdn.net/APythonC/article/details/122015076)
[React 系列小结（六）](https://blog.csdn.net/APythonC/article/details/122017918)
