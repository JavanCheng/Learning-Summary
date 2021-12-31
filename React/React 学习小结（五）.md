*所有的 Tips 均为本人 OneNote 中的笔记，复制过来就是图片，懒得转文本了*

# 第七章：redux
## 一、相关理解
### 1.1 redux是什么
1.	redux 是一个专门用于做状态管理的JS库(不是 react 插件库)
2.	它可以用在 react, angular, vue 等项目中, 但基本与 react 配合使用
3.	作用：集中式管理react应用中多个组件共享的状态
4.	redux 将组件里组员被共享的妆状态单独拎出来，有需要的组件就自己去获取
### 1.2 什么情况下需要使用 redux
1.	某个组件的状态，需要让其他组件可以随时拿到（共享）
2.	一个组件需要改变另一个组件的状态（通信）
3.	总体原则：能不用就不用, 如果不用比较吃力才考虑使用

### 1.3 redux 工作流程
![在这里插入图片描述](https://img-blog.csdnimg.cn/69994ee83a3b4c64b90b25cef8c272df.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQVB5dGhvbkM=,size_20,color_FFFFFF,t_70,g_se,x_16)
React Components：React 组件
Action Creators：动作创建
dispatch：分发动作
type：如 加减乘除
data：如 1、2、3、4
Store：仓库
Reducers：改变状态（也可以用来初始化状态，此时 action 为初始化，previousState 为 undefined）
## 二、核心概念
### 2.1 action
1.	动作的对象
2.	包含2个属性
（1）type：标识属性, 值为字符串, 唯一, 必要属性
（2）data：数据属性, 值类型任意, 可选属性
3.	例子：{ type: 'ADD_STUDENT',data:{name: 'tom',age:18} }

### 2.2 reducer
1.	用于初始化状态、加工状态
2.	加工时，根据旧的state和action， 产生新的state的纯函数（后面介绍）

### 2.3 store
1.	将state、action、reducer联系在一起的对象
2.	如何得到此对象：
（1）import {createStore} from 'redux'
（2）import reducer from './reducers'
（3）const store = createStore(reducer)
3.	此对象的功能：
（1）getState(): 得到state
（2）dispatch(action): 分发action, 触发reducer调用, 产生新的state
（3）subscribe(listener): 注册监听, 当产生了新的state时, 自动调用

## 三、核心API
### 3.1 createstore
作用：创建包含指定reducer的store对象
### 3.2 store对象
1.	作用: redux库最核心的管理对象
2.	它内部维护着:
（1）state
（2）reducer
3.	核心方法:
（1）getState()
（2）dispatch(action)
（3）subscribe(listener)
4.	具体编码:
（1）store.getState()
（2）store.dispatch({type:'INCREMENT', number})
（3）store.subscribe(render)
### 3.3 applyMiddleware
作用：应用上基于redux的中间件(插件库)
### 3.4 combineReducers
作用：合并多个reducer函数
## 四、求和案例
### 4.1 效果展示
![在这里插入图片描述](https://img-blog.csdnimg.cn/c34ef09e08d24b2f931c262953dacdd0.png)
### 4.2 纯 react 版实现
1. 结构目录如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/e40a1c66d41448ca83d16d436bb0a732.png)
2. App.jsx

```javascript
import React, { Component } from 'react'
import Count from './components/Count'

export default class App extends Component {
	render() {
		return (
			<div>
				<Count/>
			</div>
		)
	}
}
```
3. src/components/Count/index.jsx

```javascript
import React, { Component } from 'react'

export default class Count extends Component {

	state = {count:0}

	//加法
	increment = ()=>{
		const {value} = this.selectNumber
		const {count} = this.state
		this.setState({count:count+value*1})
	}
	//减法
	decrement = ()=>{
		const {value} = this.selectNumber
		const {count} = this.state
		this.setState({count:count-value*1})
	}
	//奇数再加
	incrementIfOdd = ()=>{
		const {value} = this.selectNumber
		const {count} = this.state
		if(count % 2 !== 0){
			this.setState({count:count+value*1})
		}
	}
	//异步加
	incrementAsync = ()=>{
		const {value} = this.selectNumber
		const {count} = this.state
		setTimeout(()=>{
			this.setState({count:count+value*1})
		},500)
	}

	render() {
		return (
			<div>
				<h1>当前求和为：{this.state.count}</h1>
				<select ref={c => this.selectNumber = c}>
					<option value="1">1</option>
					<option value="2">2</option>
					<option value="3">3</option>
				</select>&nbsp;
				<button onClick={this.increment}>+</button>&nbsp;
				<button onClick={this.decrement}>-</button>&nbsp;
				<button onClick={this.incrementIfOdd}>当前求和为奇数再加</button>&nbsp;
				<button onClick={this.incrementAsync}>异步加</button>&nbsp;
			</div>
		)
	}
}
```
### 4.3 redux 精简版实现
1. 结构目录：
![在这里插入图片描述](https://img-blog.csdnimg.cn/a99c722549894bb78a03aaedd449027c.png)
2. 操作步骤：
(1)去除Count组件自身的状态
(2)src下建立:
	`redux/store.js` 和 `redux/count_reducer.js`
(3)store.js：
	a. 引入redux中的createStore函数，创建一个store
	b. createStore调用时要传入一个为其服务的reducer
	c. 记得暴露store对象
```javascript
// store.js：
/* 
	该文件专门用于暴露一个store对象，整个应用只有一个store对象
*/
//引入createStore，专门用于创建redux中最为核心的store对象
import {createStore} from 'redux'
//引入为Count组件服务的reducer
import countReducer from './count_reducer'
//暴露store
//createStore创建store
export default createStore(countReducer)
```		
(4)count_reducer.js:
a. reducer的本质是一个函数，接收：preState,action，返回加工后的状态
b. reducer有两个作用：初始化状态，加工状态
c. reducer被第一次调用时，是store自动触发的，
								传递的preState是undefined,
								传递的action是:{type:'@@REDUX/INIT_a.2.b.4}
```javascript
// count_reducer.js:
/* 
	1.该文件是用于创建一个为Count组件服务的reducer，reducer的本质就是一个函数
	2.reducer函数会接到两个参数，分别为：之前的状态(preState)，动作对象(action)
*/
const initState = 0 //初始化状态
// countReducer为createStore创建，传入先前状态preState和action两个参数
export default function countReducer(preState=initState,action){
	// console.log(preState);
	//从action对象中获取：type、data
	//action存在type和data属性
	const {type,data} = action
	//根据type决定如何加工数据
	switch (type) {
		case 'increment': //如果是加
			return preState + data
		case 'decrement': //若果是减
			return preState - data
		default:
			return preState
	}
}
```
(5)在index.js中监测store中状态的改变，一旦发生改变重新渲染`<App/>`
备注：redux只负责管理状态，至于状态的改变驱动着页面的展示，要靠我们自己写

```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'
import store from './redux/store'

ReactDOM.render(<App/>,document.getElementById('root'))

// 监听状态，store监听reducer的状态改变，随后做出响应重新渲染页面
// 因 Diffing 算法存在，故效率不存在问题
store.subscribe(()=>{
	ReactDOM.render(<App/>,document.getElementById('root'))
})
```
(6)此时 Count 组件如下：

```javascript
import React, { Component } from 'react'
//引入store，用于获取redux中保存状态
import store from '../../redux/store'

export default class Count extends Component {
	state = {carName:'奔驰c63'}
	// 此方法存在效率问题
	/* componentDidMount(){
		//检测redux中状态的变化，只要变化，就调用render
		store.subscribe(()=>{
			this.setState({})
		})
	} */
	
	//加法
	increment = ()=>{
		const {value} = this.selectNumber
		//dispatch：分发action的api
		store.dispatch({type:'increment',data:value*1})
	}
	//减法
	decrement = ()=>{
		const {value} = this.selectNumber
		store.dispatch({type:'decrement',data:value*1})
	}
	//奇数再加
	incrementIfOdd = ()=>{
		const {value} = this.selectNumber
		//getState：获取初始化状态，此处为count_reducer中的initState=0
		const count = store.getState()
		if(count % 2 !== 0){
			store.dispatch({type:'increment',data:value*1})
		}
	}
	//异步加
	incrementAsync = ()=>{
		const {value} = this.selectNumber
		setTimeout(()=>{
			store.dispatch({type:'increment',data:value*1})
		},500)
	}

	render() {
		return (
			<div>
				<h1>当前求和为：{store.getState()}</h1>
				<select ref={c => this.selectNumber = c}>
					<option value="1">1</option>
					<option value="2">2</option>
					<option value="3">3</option>
				</select>&nbsp;
				<button onClick={this.increment}>+</button>&nbsp;
				<button onClick={this.decrement}>-</button>&nbsp;
				<button onClick={this.incrementIfOdd}>当前求和为奇数再加</button>&nbsp;
				<button onClick={this.incrementAsync}>异步加</button>&nbsp;
			</div>
		)
	}
}
```
### 4.4 redux 完整版实现
1. 新增文件 count_action.js 专门用于创建action对象

```javascript
/* 
	该文件专门为Count组件生成action对象
*/
import {INCREMENT,DECREMENT} from './constant'

export const createIncrementAction = data => ({type:INCREMENT,data})
export const createDecrementAction = data => ({type:DECREMENT,data})
```
>Tips:
>1. 外面包小括号表示返回一个对象
>2. 如果正常回调函数外面包花括号，并用return返回一个对象
>3. 用小括号省略return

2. 新增文件 constant.js 放置容易写错的type值

```javascript
/* 
	该模块是用于定义action对象中type类型的常量值，目的只有一个：便于管理的同时防止程序员单词写错
*/
export const INCREMENT = 'increment'
export const DECREMENT = 'decrement'
```
3. 此时 Count 的实现为：

```javascript
//奇数再加
incrementIfOdd = ()=>{
	const {value} = this.selectNumber
	const count = store.getState()
	if(count % 2 !== 0){
		store.dispatch(createIncrementAction(value*1))
	}
}
```
## 五、redux 异步编程
### 5.1 理解
1.	redux默认是不能进行异步处理的
2.	某些时候应用中需要在redux中执行异步任务(ajax, 定时器)
### 5.2 使用异步中间件
`npm install --save redux-thunk`
### 5.3 举个例子
以求和案例为例
1. 明确：延迟的动作不想交给组件自身，想交给action
2. 何时需要异步action：想要对状态进行操作，但是具体的数据靠异步任务返回
3. 具体编码：
（1）yarn add redux-thunk，并配置在store中
（2）创建action的函数不再返回一般对象，而是一个函数，该函数中写异步任务
（3）异步任务有结果后，分发一个同步的action去真正操作数据
（4）备注：异步action不是必须要写的，完全可以自己等待异步任务的结果了再去分发同步action

![在这里插入图片描述](https://img-blog.csdnimg.cn/ebe4f4b0f9f7479890e7f65f29089140.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQVB5dGhvbkM=,size_20,color_FFFFFF,t_70,g_se,x_16)
## 六、react-redux
### 6.1 理解
1.	一个react插件库
2.	专门用来简化react应用中使用redux
### 6.2 react-redux 将所有组件分成两大类
1.	UI组件
（1）只负责 UI 的呈现，不带有任何业务逻辑
（2）通过props接收数据(一般数据和函数)
（3）不使用任何 Redux 的 API
（4）一般保存在components文件夹下
2.	容器组件
（1）负责管理数据和业务逻辑，不负责UI的呈现
（2）使用 redux 的 API
（3）一般保存在containers文件夹下
### 6.3 模型图
![在这里插入图片描述](https://img-blog.csdnimg.cn/8eda94582a5d4ff0b86bae63d6664a58.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQVB5dGhvbkM=,size_20,color_FFFFFF,t_70,g_se,x_16)
### 6.4 相关API
1.	Provider：让所有组件都可以得到state数据

```javascript
<Provider store={store}>
  <App />
</Provider>
```
2. connect：用于包装 UI 组件生成容器组件

```javascript
import { connect } from 'react-redux'
  connect(
    mapStateToprops,
    mapDispatchToProps
  )(Counter)
```
3.	mapStateToprops：将外部的数据（即state对象）转换为UI组件的标签属性


```javascript
const mapStateToprops = function (state) {
  return {
    value: state
  }
}
```
4. mapDispatchToProps：将分发action的函数转换为UI组件的标签属性
### 6.5 总结
1. 明确两个概念：
（1）UI组件:不能使用任何redux的api，只负责页面的呈现、交互等。
（2）容器组件：负责和redux通信，将结果交给UI组件。
2. 如何创建一个容器组件————靠react-redux 的 connect函数
connect(mapStateToProps,mapDispatchToProps)(UI组件)
-mapStateToProps:映射状态，返回值是一个对象
-mapDispatchToProps:映射操作状态的方法，返回值是一个对象
3. 备注1：容器组件中的store是靠props传进去的，而不是在容器组件中直接引入
4. 备注2：mapDispatchToProps，也可以是一个对象
5. 渲染容器组件，ui组件被创建并包裹在容器组件中，被一起渲染

## 七、react-redux优化
1. 容器组件和UI组件整合一个文件
2. 无需自己给容器组件传递store，给`<App/>`包裹一个`<Provider store={store}>`即可
3. 使用了react-redux后也不用再自己检测redux中状态的改变了，容器组件可以自动完成这个工作。
4. mapDispatchToProps也可以简单的写成一个对象
5. 一个组件要和redux“打交道”要经过哪几步？
（1）定义好UI组件---不暴露
（2）在UI组件中通过this.props.xxxxxxx读取和操作状态
（3）引入connect生成一个容器组件，并暴露，写法如下：
	>connect(
	>state => ({key:value}), //映射状态
	>{key:xxxxxAction} //映射操作状态的方法
	>)(UI组件)

## 八、数据共享
1. 定义一个Person组件，和Count组件通过redux共享数据
2. 为Person组件编写：reducer、action，配置constant常量
3. 重点：Person的 reducer和 Count的 reducer要使用 combineReducers 进行合并，合并后的总状态是一个对象！！！
4. 交给store的是总reducer，最后注意在组件中取出状态的时候，记得“取到位”
5. 共享后可直接从 store 中取出 state
![在这里插入图片描述](https://img-blog.csdnimg.cn/3d689d16441b4755ac4ed19130f42045.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQVB5dGhvbkM=,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/60754e5eb4714a7eaa0645008fb014f3.png)
## 九、使用redux调试工具
### 9.1 安装chrome浏览器插件
![在这里插入图片描述](https://img-blog.csdnimg.cn/00a950c8c8b84d82857cb8f5c14d60ed.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQVB5dGhvbkM=,size_18,color_FFFFFF,t_70,g_se,x_16)
### 9.2 下载工具依赖包
1. `yarn add redux-devtools-extension`
2. store中进行配置

```javascript
import {composeWithDevTools} from 'redux-devtools-extension'
const store = createStore(allReducer,composeWithDevTools(applyMiddleware(thunk)))
```
## 十、纯函数和高阶函数
### 10.1 纯函数
1.	一类特别的函数: 只要是同样的输入(实参)，必定得到同样的输出(返回)
2.	必须遵守以下一些约束  
（1）不得改写参数数据
（2）不会产生任何副作用，例如网络请求，输入和输出设备
（3）不能调用Date.now()或者Math.random()等不纯的方法  
3.	redux的reducer函数必须是一个纯函数
### 10.2 高阶函数
1.	理解: 一类特别的函数
（1）情况1: 参数是函数
（2）情况2: 返回是函数
2.	常见的高阶函数: 
（1）定时器设置函数
（2）数组的forEach()/map()/filter()/reduce()/find()/bind()
（3）promise
（4）react-redux中的connect函数
3.	作用: 能实现更加动态, 更加可扩展的功能
 
## 十一、求和案例总结
1. 所有变量名字要规范，尽量触发对象的简写形式
2. reducers文件夹中，编写index.js专门用于汇总并暴露所有的reducer
3. 项目打包运行
![在这里插入图片描述](https://img-blog.csdnimg.cn/0201397bfc984a408aaa65f61f64e4aa.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQVB5dGhvbkM=,size_16,color_FFFFFF,t_70,g_se,x_16)
## React 系列小结：
[React 学习小结（一）](https://blog.csdn.net/APythonC/article/details/121954352)
[React 学习小结（二）](https://blog.csdn.net/APythonC/article/details/121960138)
[React 学习小结（三）](https://blog.csdn.net/APythonC/article/details/121978288)
[React 学习小结（四）](https://blog.csdn.net/APythonC/article/details/121984342)
[React 系列小结（五）](https://blog.csdn.net/APythonC/article/details/122015076)
[React 系列小结（六）](https://blog.csdn.net/APythonC/article/details/122017918)
