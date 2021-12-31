*所有的 Tips 均为本人 OneNote 中的笔记，复制过来就是图片，懒得转文本了*

# 第八章：react 扩展
## 一、扩展一：setState

### 1.1 setState更新状态的2种写法
1. setState(stateChange, [callback]) ------ 对象式的setState
（1）stateChange为状态改变对象(该对象可以体现出状态的更改)
（2）callback是可选的回调函数, 它在**状态更新完毕、界面也更新后(render调用后)才被调用**
					
2. setState(updater, [callback]) ------ 函数式的setState
（1）updater为返回stateChange对象的函数
（2）**updater可以接收到state和props**
（3）callback是可选的回调函数, 它在状态更新、界面也更新后(render调用后)才被调用
### 1.2 写法举例

```javascript
add = ()=>{
	//对象式的setState
	/* //1.获取原来的count值
	const {count} = this.state
	//2.更新状态
	this.setState({count:count+1},()=>{
		console.log(this.state.count);
	})
	//console.log('12行的输出',this.state.count); //0 */

	//函数式的setState
	this.setState( state => ({count:state.count+1}))
}
```
### 1.3 总结
1. 对象式的setState是函数式的setState的简写方式(语法糖)
2. 使用原则：
（1）如果新状态不依赖于原状态 => 使用对象方式
（2）如果新状态依赖于原状态 => 使用函数方式
（3）如果需要在setState()执行后获取最新的状态数据, 要在第二个callback函数中读取

## 二、扩展二：lazyLoad
### 2.1 路由组件的 lazyLoad
1. 普通使用组件在渲染页面时会把所有的组件一起渲染出来，点击切换组件
2. 如果一个页面有100个路由组件,我们只需要使用3个，则应该使用懒加载，即使用几个加载几个

### 2.2 实现

```javascript
//1.通过React的lazy函数配合import()函数动态加载路由组件 ===> 路由组件代码会被分开打包
const Login = lazy(()=>import('@/pages/Login'))
	
//2.通过<Suspense>指定在加载得到路由打包文件前显示一个自定义loading界面
<Suspense fallback={<h1>loading.....</h1>}>
      <Switch>
          <Route path="/xxx" component={Xxxx}/>
          <Redirect to="/login"/>
      </Switch>
</Suspense>
```
## 三、扩展三：Hooks
### 3.1 React Hook/Hooks是什么?
1. Hook是React 16.8.0版本增加的新特性/新语法
2. 可以让你在函数组件中使用 state 以及其他的 React 特性
3. **函数式组件性能较好，因为类式组件要创建实例，实际开发中多用函数式**
### 3.2 三个常用的Hook
1. State Hook: React.useState()
2. Effect Hook: React.useEffect()
3. Ref Hook: React.useRef()
### 3.3 State Hook
1. State Hook让函数组件也可以有state状态, 并进行状态数据的读写操作
2. 语法: const [xxx, setXxx] = React.useState(initValue)  
3. useState()说明:
（1）参数: 第一次初始化指定的值在内部作缓存（第一次指定初始值后,底层处理会让第一次调用时不会重新初始化）
（2）返回值: 包含2个元素的数组, 第1个为内部当前状态值, 第2个为更新状态值的函数
4. setXxx()的2种写法:
（1）setXxx(newValue): 参数为非函数值, 直接指定新的状态值, 内部用其覆盖原来的状态值
（2）setXxx(value => newValue): 参数为函数, 接收原本的状态值, 返回新的状态值, 内部用其覆盖原来的状态值
### 3.4 Effect Hook
1. Effect Hook 可以让你在函数组件中执行副作用操作(用于模拟类组件中的生命周期钩子)
2. React中的副作用操作:
（1）发ajax请求数据获取
（2）设置订阅 / 启动定时器
（3）手动更改真实DOM
3. 语法和说明: 
```javascript
useEffect(() => { 
     // 在此可以执行任何带副作用操作
     return () => { // 在组件卸载前执行
        // 在此做一些收尾工作, 比如清除定时器/取消订阅等
     }
}, [stateValue]) // 如果指定的是[], 回调函数只会在第一次render()后执行
```

4. 可以把 useEffect Hook 看做如下三个函数的组合
        componentDidMount()
        componentDidUpdate()
    	componentWillUnmount()
### 3.5 Ref Hook
1. Ref Hook可以在函数组件中存储/查找组件内的标签或任意其它数据
2. 语法: `const refContainer = useRef()`
3. 作用：保存标签对象,功能与React.createRef()一样

### 3.6 类式组件对比函数组件
1. 类式组件写法

```javascript
//类式组件
class Demo extends React.Component {

	state = {count:0}

	myRef = React.createRef()

	add = ()=>{
		this.setState(state => ({count:state.count+1}))
	}

	unmount = ()=>{
		ReactDOM.unmountComponentAtNode(document.getElementById('root'))
	}

	show = ()=>{
		alert(this.myRef.current.value)
	}

	componentDidMount(){
		this.timer = setInterval(()=>{
			this.setState( state => ({count:state.count+1}))
		},1000)
	}

	componentWillUnmount(){
		clearInterval(this.timer)
	}

	render() {
		return (
			<div>
				<input type="text" ref={this.myRef}/>
				<h2>当前求和为{this.state.count}</h2>
				<button onClick={this.add}>点我+1</button>
				<button onClick={this.unmount}>卸载组件</button>
				<button onClick={this.show}>点击提示数据</button>
			</div>
		)
	}
} 
```
2. 函数组件

```javascript
function Demo(){

	const [count,setCount] = React.useState(0)
	const myRef = React.useRef()

	React.useEffect(()=>{
		// 返回函数的回调相当于componentWilUnmount
		let timer = setInterval(()=>{
			setCount(count => count+1 )
		},1000)
		return ()=>{
			clearInterval(timer)
		}
	},[])
	// 第二个参数：[]
	// 不加数组方括号时，类似componentDidMount和componentDidUpdate, 组件加载和更新时都会调用回调函数
	// 加数组方括号时，如果是空白，则类似componentDidMount, 只在加载时调用一次回调
	// 加数组方括号时，如果不是空白，则监听括号里的内容，当括号里的状态发生改变时，调用一次回调

	//加的回调
	function add(){
		//setCount(count+1) //第一种写法
		setCount(count => count+1 )
	}

	//提示输入的回调
	function show(){
		alert(myRef.current.value)
	}

	//卸载组件的回调
	function unmount(){
		ReactDOM.unmountComponentAtNode(document.getElementById('root'))
	}

	return (
		<div>
			<input type="text" ref={myRef}/>
			<h2>当前求和为：{count}</h2>
			<button onClick={add}>点我+1</button>
			<button onClick={unmount}>卸载组件</button>
			<button onClick={show}>点我提示数据</button>
		</div>
	)
}
```
## 四、扩展四：Fragment
### 4.1 使用
```javascript
import React, { Component,Fragment } from 'react'

export default class Demo extends Component {
	render() {
		return (
			<Fragment key={1}>
				<input type="text"/>
				<input type="text"/>
			</Fragment>
		)
	}
}
```
### 4.2 作用
1. 可以不用必须有一个真实的DOM根标签了
2. 使用Fragment或空标签可以省略div
3. 区别是Fragment可以传入key值，而空标签什么都不能传

## 五、扩展五：Context
### 5.1 理解
一种组件间通信方式, 常用于【祖组件】与【后代组件】间通信
### 5.2 使用

```javascript
1) 创建Context容器对象：
	const XxxContext = React.createContext()  
	
2) 渲染子组时，外面包裹xxxContext.Provider, 通过value属性给后代组件传递数据：
	<xxxContext.Provider value={数据}>
		子组件
    </xxxContext.Provider>
    
3) 后代组件读取数据：

	//第一种方式:仅适用于类组件 
	  static contextType = xxxContext  // 声明接收context
	  this.context // 读取context中的value数据
	  
	//第二种方式: 函数组件与类组件都可以
	  <xxxContext.Consumer>
	    {
	      value => ( // value就是context中的value数据
	        要显示的内容
	      )
	    }
	  </xxxContext.Consumer>
```
### 5.3 注意
在应用开发中一般不用 context, 一般都用它封装的react插件
## 六、扩展六：组件优化
### 6.1 Component的2个问题
1. 只要执行setState(),即使不改变状态数据, 组件也会重新render()
2. 只要当前组件重新render(), 就会自动重新render子组件 ==> 效率低
### 6.2 效率高的做法
只有当组件的state或props数据发生改变时才重新render()
### 6.3 原因
Component 中的 shouldComponentUpdate() 总是返回true
### 6.4 解决
1. 办法1: 
	重写 shouldComponentUpdate() 方法
	比较新旧state或props数据, 如果有变化才返回true, 如果没有返回false
2. 办法2:  
	使用 PureComponent
	PureComponent 重写了 shouldComponentUpdate(), 只有state或props数据有变化才返回true
3. 注意: 
	（1）PureComponent 只是进行state和props数据的浅比较, 如果只是数据对象内部数据变了, 返回false  
	（2）不要直接修改state数据, 而是要产生新数据
	（3）项目中一般使用PureComponent来优化

### 6.5 PureComponent 的使用
```javascript
import React, { PureComponent } from 'react'
import './index.css'

export default class Parent extends PureComponent {
}
```
## 七、扩展七：render props
### 7.1 如何向组件内部动态传入带内容的结构(标签)
1. Vue中: 
	使用slot技术, 也就是通过组件标签体传入结构  <AA><BB/></AA>
2. React中:
	使用children props: 通过组件标签体传入结构
	使用render props: 通过组件标签属性传入结构, 一般用render函数属性
### 7.2 children props
1. 使用：

```javascript
<A>
  <B>xxxx</B>
</A>
{this.props.children}
```
2. 问题: 如果B组件需要A组件内的数据 ==> 做不到 
### 7.3 render props
1. 使用

```javascript
<A render={(data) => <C data={data}></C>}></A>
```

A组件: {this.props.render(内部state数据)}
C组件: 读取A组件传入的数据显示 {this.props.data} 
## 八、扩展八：错误边界
### 8.1 理解
错误边界：用来捕获后代组件错误，渲染出备用页面
### 8.2 特点
只能捕获后代组件生命周期产生的错误，不能捕获自己组件产生的错误和其他组件在合成事件、定时器中产生的错误
### 8.3 使用方式
getDerivedStateFromError 配合 componentDidCatch

```javascript
// 生命周期函数，一旦后台组件报错，就会触发
static getDerivedStateFromError(error) {
    console.log(error);
    // 在render之前触发
    // 返回新的state
    return {
        hasError: true,
    };
}

componentDidCatch(error, info) {
    // 统计页面的错误。发送请求发送到后台去
    console.log(error, info);
}
```
## 九、扩展九：组件间的通信方式总结
### 9.1 组价间的关系
1. 父子组件
2. 兄弟组件（非嵌套组件）
3. 祖孙组件（跨级组件）
### 9.2 几种通信方式
1. props
（1）children props
（2）render props
2. 消息订阅-发布
pubs-sub、event 等等
3. 集中式管理
redux、dva 等等
4. context
生产者-消费者模式
### 9.3 比较好的搭配方式
1. 父子组件：props
2. 兄弟组件(非嵌套组件)：消息订阅-发布、集中式管理
3. 祖孙组件(跨级组件)：消息订阅-发布、集中式管理、context(用的少)











## React 系列小结：
[React 学习小结（一）](https://blog.csdn.net/APythonC/article/details/121954352)
[React 学习小结（二）](https://blog.csdn.net/APythonC/article/details/121960138)
[React 学习小结（三）](https://blog.csdn.net/APythonC/article/details/121978288)
[React 学习小结（四）](https://blog.csdn.net/APythonC/article/details/121984342)
[React 系列小结（五）](https://blog.csdn.net/APythonC/article/details/122015076)
[React 系列小结（六）](https://blog.csdn.net/APythonC/article/details/122017918)
