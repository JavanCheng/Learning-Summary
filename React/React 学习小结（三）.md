*所有的 Tips 均为本人 OneNote 中的笔记，复制过来就是图片，懒得转文本了*

# 第三章：React 应用（基于React脚手架）
## 一、使用 create-react-app 创建 react 应用
### 1.1 react 脚手架
1.	xxx脚手架: 用来帮助程序员快速创建一个基于xxx库的模板项目
	1.包含了所有需要的配置（语法检查、jsx编译、devServer…）
	2.下载好了所有相关的依赖
	3.可以直接运行一个简单效果
2.	react提供了一个用于创建react项目的脚手架库: create-react-app
3.	项目的整体技术架构为:  react + webpack + es6 + eslint
4.	使用脚手架开发的项目的特点: 模块化, 组件化, 工程化

### 1.2 创建项目并启动
1. 第一步，全局安装：npm i -g create-react-app
2. 第二步，切换到想创项目的目录，使用命令：create-react-app hello-react （hello-react 为项目名）
3. 第三步，进入项目文件夹：cd hello-react
4. 第四步，启动项目：npm start

### 1.3 react 脚手架项目结构
![在这里插入图片描述](https://img-blog.csdnimg.cn/a1373a32bbc0417695cfaa5a39ba7ed8.png)
```
public ---- 静态资源文件夹
		favicon.icon ------ 网站页签图标
		index.html -------- 主页面
		logo192.png ------- logo图
		logo512.png ------- logo图
		manifest.json ----- 应用加壳的配置文件
		robots.txt -------- 爬虫协议文件
src ---- 源码文件夹
		App.css -------- App组件的样式
		App.js --------- App组件
		App.test.js ---- 用于给App做测试
		index.css ------ 样式
		index.js ------- 入口文件
		logo.svg ------- logo图
		reportWebVitals.js
			--- 页面性能分析文件(需要web-vitals库的支持)
		setupTests.js
			---- 组件单元测试的文件(需要jest-dom库的支持)
```
### 1.4 功能界面的组件化编码流程
1. 拆分组件: 拆分界面,抽取组件
2. 实现静态组件: 使用组件实现静态页面效果
3. 实现动态组件
	1.动态显示初始化数据
		——数据类型
		——数据名称
		——保存在哪个组件
	2.交互(从绑定事件监听开始)

>Tips：样式的模块化
>1. 将 index.css 文件重命名为 index.module.css ，并在引用时用以下形式引入：
>`<h2 className={hello.title}>Hello,React!</h2>`
>2. 用 less 写可以不做样式模块化

>Tips：标识组件的方法
>1. 首字母大写
>2. 后缀改为 .jsx
>3. 若组件内的 .jsx 为 index.jsx，则 import 导入时可以只写到文件夹
## 二、实战案例
### 2.1 ToDoList案例
[TodoList案例](https://blog.csdn.net/APythonC/article/details/121982246)
### 2.2 全选反选案例
[全选反选框](https://blog.csdn.net/APythonC/article/details/118996740)

# 第四章、React ajax
## 一、理解 React ajax
### 1.1 前置说明
1.	React 本身只关注于界面, 并不包含发送 ajax 请求的代码
2.	前端应用需要通过 ajax 请求与后台进行交互 (json数据)
3.	React 应用中需要集成第三方 ajax 库(或自己封装)
### 1.2 常用的 ajax 请求库
1.	jQuery: 比较重, 如果需要另外引入不建议使用
2.	axios: 轻量级, 建议使用
（1）封装XmlHttpRequest对象的ajax
（2）promise风格
（3）可以用在浏览器端和node服务器端

## 二、axios
### 2.1 文档地址
[axios](https://github.com/axios/axios)
### 2.2 相关 API
1. GET 请求

```javascript
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response.data);
  })
  .catch(function (error) {
    console.log(error);
  });

axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

```
2. POST 请求
```javascript
axios.post('/user', {
  firstName: 'Fred',
  lastName: 'Flintstone'
})
.then(function (response) {
console.log(response);
})
.catch(function (error) {
console.log(error);
});
```
>Tips：网络请求三要素
>1. url —— 路径
>2. method —— 方式
>3. params —— 参数
## 三、react脚手架配置代理总结
### 3.1 方法一
在package.json中追加如下配置

```json
"proxy":"http://localhost:5000"
```

说明：

1. 优点：配置简单，前端请求资源时可以不加任何前缀。
2. 缺点：不能配置多个代理。
3. 工作方式：上述方式配置代理，当请求了3000不存在的资源时，那么该请求会转发给5000 （优先匹配前端资源）

### 3.2 方法二
1. 第一步：创建代理配置文件

   ```
   在src下创建配置文件：src/setupProxy.js
   ```

2. 编写setupProxy.js配置具体代理规则：

   ```js
   const proxy = require('http-proxy-middleware')
   
   module.exports = function(app) {
     app.use(
       proxy('/api1', {  //api1是需要转发的请求(所有带有/api1前缀的请求都会转发给5000)
         target: 'http://localhost:5000', //配置转发目标地址(能返回数据的服务器地址)
         changeOrigin: true, //控制服务器接收到的请求头中host字段的值
         /*
         	changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
         	changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:3000
         	changeOrigin默认值为false，但我们一般将changeOrigin值设为true
         */
         pathRewrite: {'^/api1': ''} //去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置)
       }),
       proxy('/api2', { 
         target: 'http://localhost:5001',
         changeOrigin: true,
         pathRewrite: {'^/api2': ''}
       })
     )
   }
   ```
说明：
1. 优点：可以配置多个代理，可以灵活的控制请求是否走代理。
2. 缺点：配置繁琐，前端请求资源时必须加前缀。

## 四、消息订阅-发布机制
1.	工具库: PubSubJS
2.	下载: npm install pubsub-js --save
3.	使用: 
（1）import PubSub from 'pubsub-js' //引入
（2）PubSub.subscribe('delete', function(data){ }); //订阅
（3）PubSub.publish('delete', data) //发布消息

```javascript
// 发送消息：PubSub.publish(名称,参数)
PubSub.publish('atguigu',{isFirst:false,isLoading:true})

// 订阅消息：var token=PubSub.subscribe(名称,函数)
componentDidMount(){
    this.token = PubSub.subscribe('atguigu',(_,stateObj)=>{ //_下划线占位
    this.setState(stateObj)
  })
}
// 取消订阅：PubSub.unsubscribe(token)
componentWillUnmount(){
    PubSub.unsubscribe(this.token)
}
```

4. 功能：可以实现子组件间的通信，而不用通过父组件传递参数
5. 优点
（1）低耦合
publisher和subscriber时低耦合的，他们可以继续运行而不用管对方的状态。在传统的客服端-服务器范例中，服务器不运行，客户端就不能发送消息到服务器。客户端不运行，服务器就接收不到消息。
（2）可扩展型
通过平行的运行机制，消息缓存机制，基于一定规则的消息发送机，相比通常的客户端-服务端模式，订阅发布模式有更好的扩展性。但是在高耦合大容量的企业级环境中，不适合订阅发布机制。
6. 缺点
低耦合
为什么这么说呢？publisher的消息数据结构必须定义的很合理，有相当的伸缩性。为了更改要发布的消息数据结构，publisher可能要知道所有的subscriber，同时也要修改他们或者与旧版本保持兼容。这使得重构publisher变的很困难。因为随着时间推移可能会更改需求，publisher数据结构的不变性会成为负担

## 五、Fetch
### 5.1 文档
1. [github](https://github.github.io/fetch/)
2. [segmentfault](https://segmentfault.com/a/1190000003810652)

### 5.2 特点
1.	fetch: 原生函数，不再使用XmlHttpRequest对象提交ajax请求
2.	兼容性存在问题，老版本浏览器可能不支持
3. 遵循关注分离原则，比如下楼买东西，拆解成下楼 + 买东西
### 5.3 相关 API
1. GET 请求

```javascript
fetch(url).then(function(response) {
    return response.json()
  }).then(function(data) {
    console.log(data)
  }).catch(function(e) {
    console.log(e)
  });
```
2. POST 请求

```javascript
fetch(url, {
    method: "POST",
    body: JSON.stringify(data),
  }).then(function(data) {
    console.log(data)
  }).catch(function(e) {
    console.log(e)
  })
```

>Tips：与 xhr 的区别
>![在这里插入图片描述](https://img-blog.csdnimg.cn/c27f3ab8aa83450ca84c37c10d7bfed8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQVB5dGhvbkM=,size_13,color_FFFFFF,t_70,g_se,x_16)

## 六、github 搜索案例要点
1. 设计状态时要考虑全面，例如带有网络请求的组件，要考虑请求失败怎么办。
2. ES6小知识点：解构赋值+重命名

```javascript
let obj = {a:{b:1}}
const {a} = obj; //传统解构赋值
const {a:{b}} = obj; //连续解构赋值
const {a:{b:value}} = obj; //连续解构赋值+重命名
```
3. 消息订阅与发布机制
（1）先订阅，再发布（理解：有一种隔空对话的感觉）
（2）适用于任意组件间通信
（3）要在组件的componentWillUnmount中取消订阅
4. fetch发送请求（关注分离的设计思想）

```javascript
try {
	const response= await fetch(`/api1/search/users2?q=${keyWord}`)
	const data = await response.json()
	console.log(data);
} catch (error) {
	console.log('请求出错',error);
}
```





## React 系列小结：
[React 学习小结（一）](https://blog.csdn.net/APythonC/article/details/121954352)
[React 学习小结（二）](https://blog.csdn.net/APythonC/article/details/121960138)
[React 学习小结（三）](https://blog.csdn.net/APythonC/article/details/121978288)
[React 学习小结（四）](https://blog.csdn.net/APythonC/article/details/121984342)
[React 系列小结（五）](https://blog.csdn.net/APythonC/article/details/122015076)
[React 系列小结（六）](https://blog.csdn.net/APythonC/article/details/122017918)
