*所有的 Tips 均为本人 OneNote 中的笔记，复制过来就是图片，懒得转文本了*

# 第二章：React 面向组件编程
## 一、基本理解和使用
### 1.1 使用React开发者工具调试
![在这里插入图片描述](https://img-blog.csdnimg.cn/d771fc00b30c4af39d66a170d616f4ae.png)
### 1.2 函数式组件

```javascript
//1.创建函数式组件
function MyComponent(){
	console.log(this); //此处的this是undefined，因为babel编译后开启了严格模式
	return <h2>我是用函数定义的组件(适用于【简单组件】的定义)</h2>
}
//2.渲染组件到页面
ReactDOM.render(<MyComponent/>,document.getElementById('test'))
```
执行了 ReactDOM.render(）之后，发生了什么？
1. React解析组件标签，找到了MyComponent组件
2. 发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中
### 1.3 类式组件

```javascript
//1.创建类式组件
class MyComponent extends React.Component {
	render(){
		// 类中所定义的方法，都是放在了类的原型对象上，供实例去使用
		//render是放在哪里的？—— MyComponent的原型对象上，供实例使用。
		//render中的this是谁？—— MyComponent的实例对象 <=> MyComponent组件实例对象。
		console.log('render中的this:',this);
		return <h2>我是用类定义的组件(适用于【复杂组件】的定义)</h2>
	}
}
//2.渲染组件到页面
ReactDOM.render(<MyComponent/>,document.getElementById('test'))
```
执行了 ReactDOM.render(）之后，发生了什么？
1. React解析组件标签，找到了MyComponent组件
2. 发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法
3. 将render返回的虚拟DOM转为真实DOM，随后呈现在页面中
### 1.4 注意要点
1. 组件名必须首字母大写
2. 虚拟 DOM 元素只能有一个根元素
3. 虚拟 DOM 元素必须有结束标签
4. 类中所定义的方法，都是放在了类的原型对象上，供实例去使用
5. 类中的方法默认开启了局部的严格模式，所以 this 指向为 undefined
### 1.5 渲染类组件标签的基本流程
1.	React 内部会创建组件实例对象
2.	调用 render() 得到虚拟 DOM , 并解析为真实 DOM
3.	插入到指定的页面元素内部


## 二、组件实例三大核心属性1 —— state
### 2.1 举个例子

```javascript
//1.创建组件
class Weather extends React.Component{
	//初始化状态
	state = {isHot:false,wind:'微风'}

	render(){
		// 解构赋值获取 state
		const {isHot,wind} = this.state
		return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}，{wind}</h1>
	}

	//自定义方法————要用赋值语句的形式+箭头函数
	changeWeather = () => {
		const isHot = this.state.isHot
		this.setState({isHot:!isHot})
	}
}
//2.渲染组件到页面
ReactDOM.render(<Weather/>,document.getElementById('test'))
```

### 2.2 理解 state
1.	state是组件对象最重要的属性, 值是对象 (可以包含多个key-value的组合)
2.	组件被称为"状态机", 通过更新组件的state来更新对应的页面显示 (重新渲染组件)

### 2.3 强烈注意
1.	组件中 render 方法中的 this 为组件实例对象
2.	组件自定义的方法中 this 为undefined，如何解决？
（1）强制绑定this: 通过函数对象的 bind()
（2）箭头函数（箭头函数中的 this 指向外层，此处指向 Weather）
3.	状态数据，不能直接修改或更新
4.	严重注意：状态必须通过 **setState** 进行更新,且更新是一种合并，不是替换
5.	这是错误的写法：this.state.isHot = !isHot 

## 三、组件实例三大核心属性2 —— props
函数式组件没有 state 和 resf 属性，只有 props
### 3.1 举个例子

```javascript
//创建组件
class Person extends React.Component{
	constructor(props){
		//构造器是否接收props，是否传递给super，取决于：是否希望在构造器中通过this访问props
		//一般构造器可省略
		super(props)
		console.log('constructor',this.props);
	}

	//对标签属性进行类型、必要性的限制
	static propTypes = {
		name:PropTypes.string.isRequired, //限制name必传，且为字符串
		sex:PropTypes.string,//限制sex为字符串
		age:PropTypes.number,//限制age为数值
	}

	//指定默认标签属性值
	static defaultProps = {
		sex:'男',//sex默认值为男
		age:18 //age默认值为18
	}
			
	render(){
		const {name,age,sex} = this.props
		//props是只读的
		//this.props.name = 'jack' //此行代码会报错，因为props是只读的
		return (
			<ul>
				<li>姓名：{name}</li>
				<li>性别：{sex}</li>
				<li>年龄：{age+1}</li>
			</ul>
		)
	}
}
//渲染组件到页面
ReactDOM.render(<Person name="jerry"/>,document.getElementById('test1'))
// {...p}为批量传递props，也叫批量传递标签属性，展开运算符不能展开对象，但引入react文件，写babel代码，则可以使用展开运算符展开
// const p = {name:'老刘',age:18,sex:'女'}
// ReactDOM.render(<Person {...p}/>,document.getElementById('test3'))
```
### 3.2 理解 props
1.	每个组件对象都会有 props(properties的简写) 属性
2.	组件标签的所有属性都保存在 props 中
### 3.3 作用
1.	通过标签属性从组件外向组件内传递变化的数据
2.	注意: 组件内部不要修改props数据

### 3.4 函数组件使用 props

```javascript
//创建组件
function Person (props){
	const {name,age,sex} = props
	return (
			<ul>
				<li>姓名：{name}</li>
				<li>性别：{sex}</li>
				<li>年龄：{age}</li>
			</ul>
	)
}
Person.propTypes = {
	name:PropTypes.string.isRequired, //限制name必传，且为字符串
	sex:PropTypes.string,//限制sex为字符串
	age:PropTypes.number,//限制age为数值
}
//指定默认标签属性值
Person.defaultProps = {
	sex:'男',//sex默认值为男
	age:18 //age默认值为18
}
//渲染组件到页面
ReactDOM.render(<Person name="jerry"/>,document.getElementById('test1'))
```
## 四、组件实例三大核心属性3 —— refs
### 4.1 举个例子

```javascript
//创建组件
class Demo extends React.Component{
	//展示左侧输入框的数据
	//字符串形式的ref
	showData = ()=>{
		const {input1} = this.refs
		alert(input1.value)
	}
	//展示右侧输入框的数据
	//回调形式的ref
	showData2 = ()=>{
		const {input2} = this
		alert(input2.value)
	}
	render(){
		return(
			<div>
				<input ref="input1" type="text" placeholder="点击按钮提示数据"/>&nbsp;
				<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
				<input ref={c => this.input2 = c } onBlur={this.showData2} type="text" placeholder="失去焦点提示数据"/>
			</div>
		)
	}
}
//渲染组件到页面
ReactDOM.render(<Demo/>,document.getElementById('test'))
```
### 4.2 理解 refs
组件内的标签可以定义ref属性来标识自己
### 4.3 编码
1. 字符串形式的 ref

```javascript
<input ref="input1"/>
```
2. 回调形式的 ref

```javascript
// 将当前节点放在组件实例自身上，并给他起了个名字叫 input1
// c 是 currentNode 的意思
<input ref = c => this.input1 = c />
```

3. createRef 创建 ref 容器

```javascript
// React.createRef调用后可以返回一个容器，该容器可以存储被ref所标识的节点,该容器是“专人专用”的
class Demo extends React.Component{
	//用几个就得创建几个
	myRef = React.createRef()
	myRef2 = React.createRef()
	//展示左侧输入框的数据
	showData = ()=>{
		// current 是容器内的 key
		alert(this.myRef.current.value);
	}
	//展示右侧输入框的数据
	showData2 = ()=>{
		alert(this.myRef2.current.value);
	}
	render(){
		return(
			<div>
				<input ref={this.myRef} type="text" placeholder="点击按钮提示数据"/>&nbsp;
				<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
				<input onBlur={this.showData2} ref={this.myRef2} type="text" placeholder="失去焦点提示数据"/>&nbsp;
			</div>
		)
	}
}
//渲染组件到页面
ReactDOM.render(<Demo/>,document.getElementById('test'))
```

### 4.4 注意要点
1. 内联形式的回调和类绑定形式的回调功能没区别，内联在更新时会多调用一次，可以直接用
2. 字符串形式的 ref 效率不高
3. createRef 创建容器较为繁琐，用一个就得创建一个，所以要少用

## 五、事件处理
1.	通过 onXxx 属性指定事件处理函数(注意大小写)
（1）React 使用的是自定义(合成)事件, 而不是使用的原生 DOM 事件 —————— 为了更好的兼容性
（2）React 中的事件是通过事件委托方式处理的(委托给组件最外层的元素) ————————为了高效
2.	通过 event.target 得到发生事件的 DOM 元素对象 ——————————不要过度使用 ref

```javascript
showData2 = (event)=>{
	alert(event.target.value);
}
```
## 六、收集表单数据
### 6.1 受控组件
特性：将输入存入状态（state），用时再取（可以省略 ref）

```javascript
//创建组件
class Login extends React.Component{
	//初始化状态
	state = {
		username:'', //用户名
		password:'' //密码
	}
	//保存用户名到状态中
	saveUsername = (event)=>{
		this.setState({username:event.target.value})
	}
	//保存密码到状态中
	savePassword = (event)=>{
		this.setState({password:event.target.value})
	}
	//表单提交的回调
	handleSubmit = (event)=>{
		event.preventDefault() //阻止表单提交
		const {username,password} = this.state
		alert(`你输入的用户名是：${username},你输入的密码是：${password}`)
	}
	render(){
		return(
			<form onSubmit={this.handleSubmit}>
				用户名：<input onChange={this.saveUsername} type="text" name="username"/>
				密码：<input onChange={this.savePassword} type="password" name="password"/>
				<button>登录</button>
			</form>
		)
	}
}
//渲染组件
ReactDOM.render(<Login/>,document.getElementById('test'))
```

### 6.2 非受控组件
特性：输入现用现取

```javascript
//创建组件
class Login extends React.Component{
	handleSubmit = (event)=>{
		event.preventDefault() //阻止表单提交
		const {username,password} = this
		alert(`你输入的用户名是：${username.value},你输入的密码是：${password.value}`)
	}
	render(){
		return(
			<form onSubmit={this.handleSubmit}>
				用户名：<input ref={c => this.username = c} type="text" name="username"/>
				密码：<input ref={c => this.password = c} type="password" name="password"/>
				<button>登录</button>
			</form>
		)
	}
}
//渲染组件
ReactDOM.render(<Login/>,document.getElementById('test'))
```
## 七、高阶函数 —— 函数柯里化
高阶函数：如果一个函数符合下面2个规范中的任何一个，那该函数就是高阶函数
1. 若A函数，接收的参数是一个函数，那么A就可以称之为高阶函数
2. 若A函数，调用的返回值依然是一个函数，那么A就可以称之为高阶函数

常见的高阶函数有：Promise、setTimeout、arr.map()等等

函数的柯里化：通过**函数调用继续返回函数**的方式，实现多次接收参数最后统一处理的函数编码形式
```javascript
function sum(a){
	return(b)=>{
		return (c)=>{
			return a+b+c
		}
	}
}
```
以上面的收集表单数据为例：

```javascript
//保存表单数据到状态中
// 根据 data 的类型返回 event.target.value
saveFormData = (dataType)=>{
	return (event)=>{
		this.setState({[dataType]:event.target.value})
	}
}

// 不用柯里化的写法
saveFormData = (dataType,event)=>{
	this.setState({[dataType]:event.target.value})
}
```
## 八、组件的生命周期
### 8.1 理解生命周期
1. 组件从创建到死亡它会经历一些特定的阶段
2. React 组件中包含一系列钩子函数(生命周期回调函数，可以理解为在需要的时候将这个函数钩出来), 会在特定的时刻调用（生命周期回调函数 <=> 生命周期钩子函数 <=> 生命周期函数 <=> 生命周期钩子）
4.	我们在定义组件时，会在特定的生命周期回调函数中，做特定的工作
### 8.2 生命周期流程图（旧）
<img src="https://img-blog.csdnimg.cn/415375282fb2462490e01cceae50ae77.png" width="70%"/>


生命周期的三个阶段（旧）
1. 初始化阶段: 由 ReactDOM.render() 触发---初次渲染
	1.constructor()：构造器
	2.componentWillMount()：组件将要挂载的钩子
	3.render()：必须使用的一个
	**4.componentDidMount()：组件挂载完毕的钩子**，一般在这个钩子中做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息
2. 更新阶段: 由组件内部 this.setSate() 或父组件重新 render 触发
	1.shouldComponentUpdate()：控制组件更新的“阀门”，返回值为true则继续进行，false则直接打断，如果不写，底层默认返回值为true

	2.componentWillUpdate()：组件将要更新的钩子
	3.render()
	4.componentDidUpdate()：组件更新完毕的钩子
3. 卸载组件: 由 ReactDOM.unmountComponentAtNode() 触发
	**1.componentWillUnmount()：组件将要卸载的钩子**，一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消息
### 8.3 生命周期（旧）举例

```javascript
//创建组件
class Count extends React.Component{
	//构造器
	constructor(props){
		console.log('Count---constructor');
		super(props)
		//初始化状态
		this.state = {count:0}
	}
	//加1按钮的回调
	add = ()=>{
		//获取原状态
		const {count} = this.state
		//更新状态
		this.setState({count:count+1})
	}

	//卸载组件按钮的回调
	death = ()=>{
		ReactDOM.unmountComponentAtNode(document.getElementById('test'))
	}
	//强制更新按钮的回调
	force = ()=>{
		this.forceUpdate()
	}
	//组件将要挂载的钩子
	componentWillMount(){
		console.log('Count---componentWillMount');
	}
	//组件挂载完毕的钩子
	componentDidMount(){
		console.log('Count---componentDidMount');
	}
	//组件将要卸载的钩子
	componentWillUnmount(){
		console.log('Count---componentWillUnmount');
	}
	//控制组件更新的“阀门”
	shouldComponentUpdate(){
		console.log('Count---shouldComponentUpdate');
		return true
	}
	//组件将要更新的钩子
	componentWillUpdate(){
		console.log('Count---componentWillUpdate');
	}
	//组件更新完毕的钩子
	componentDidUpdate(){
		console.log('Count---componentDidUpdate');
	}
	render(){
		console.log('Count---render');
		const {count} = this.state
		return(
			<div>
				<h2>当前求和为：{count}</h2>
				<button onClick={this.add}>点我+1</button>
				<button onClick={this.death}>卸载组件</button>
				<button onClick={this.force}>不更改任何状态中的数据，强制更新一下</button>
			</div>
		)
	}
}
```
### 8.4 生命周期流程图（新）
<img src="https://img-blog.csdnimg.cn/45e2436f312642a1a7c982febc285bc3.png" width="70%"/>

新旧生命周期区别：
1. 废除了旧生命周期的三个钩子: componentWillMount、 componentWillReceiveProps 和
componentWillUpdata
2. 新增了俩钩子: getDerivedStateFromProps 和 getSnapshotBeforeUpdate
	1.getDerivedStateFromProps：从 props 中获取派生的 state，即 state 值在任何时候都取决于 props
	2.getSnapshotBeforeUpdate：在组件更新前获取一次快照，返回值可以是任意类型
### 8.5 重要的钩子
1.	render：初始化渲染或更新渲染调用
2.	componentDidMount：开启监听, 发送 ajax 请求
3.	componentWillUnmount：做一些收尾工作, 如: 清理定时器
### 8.6 即将废弃的钩子
1.	componentWillMount
2.	componentWillReceiveProps
3.	componentWillUpdate

这几个钩子都带有 will，现在使用会出现警告，下一个大版本需要加上UNSAFE_前缀才能使用，以后可能会被彻底废弃，不建议使用
## 九、虚拟 DOM 与 DOM Diffing 算法
### 9.1 基本原理图
![在这里插入图片描述](https://img-blog.csdnimg.cn/15280141120a4d4b97f9050805b82cf4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQVB5dGhvbkM=,size_20,color_FFFFFF,t_70,g_se,x_16)
### 9.2 补充：key 的作用
经典面试题:
1. react/vue 中的 key 有什么作用？（key 的内部原理是什么？）
2. 为什么遍历列表时，key 最好不要用 index? 
```
1.虚拟DOM中key的作用：
	（1）简单的说: key是虚拟DOM对象的标识, 在更新显示时key起着极其重要的作用
	（2）详细的说: 当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】, 随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：
		a. 旧虚拟DOM中找到了与新虚拟DOM相同的key：
			a1.若虚拟DOM中内容没变, 直接使用之前的真实DOM
			a2.若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM
		b. 旧虚拟DOM中未找到与新虚拟DOM相同的key：
			根据数据创建新的真实DOM，随后渲染到到页面

2.用index作为key可能会引发的问题：
	（1）若对数据进行：逆序添加、逆序删除等破坏顺序操作:
		会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低
	（2）如果结构中还包含输入类的DOM：
		会产生错误DOM更新 ==> 界面有问题
	（3）注意！如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的

3.开发中如何选择key：
	（1）最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值
	（2）如果确定只是简单的展示数据，用index也是可以的
```


## React 系列小结：
[React 学习小结（一）](https://blog.csdn.net/APythonC/article/details/121954352)
[React 学习小结（二）](https://blog.csdn.net/APythonC/article/details/121960138)
[React 学习小结（三）](https://blog.csdn.net/APythonC/article/details/121978288)
[React 学习小结（四）](https://blog.csdn.net/APythonC/article/details/121984342)
[React 系列小结（五）](https://blog.csdn.net/APythonC/article/details/122015076)
[React 系列小结（六）](https://blog.csdn.net/APythonC/article/details/122017918)
