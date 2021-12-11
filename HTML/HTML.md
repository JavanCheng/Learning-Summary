# HTML 学习小结

## 一、基本概念

### 1. 网页

<span style="color:red">网页是构成网站的基本元素</span>，它通常由图片、链接、文字、声音、视频等元素组成。通常我们看到的网页，常见以 .htm 或 .html 后缀结尾的文件，因此将其俗称为<span style="color:red"> HTML 文件</span>

### 2. HTML
HTML 指的是<span style="color:red">超文本标记语言 (Hyper Text Markup Language) </span>，它是用来描述网页的一种语言
HTML 不是一种编程语言，而是一种标记语言 (markup language)
标记语言是一套标记标签 (markup tag)
<span style="color:red">超文本有 2 层含义：</span>

1. 它可以加入图片、声音、动画、多媒体等内容（超越了文本限制 ）
2. 它还可以从一个文件跳转到另一个文件，与世界各地主机的文件连接（超级链接文本 ）

### 3. Web结构的构成
主要包括<span style="color:red">结构（Structure）、表现（Presentation）和行为（Behavior）</span>三个方面
1. 结构：HTML

2. 表现：CSS

3. 行为：JavaScript

  ![image-20211208203339089](C:\Users\程玉峰\AppData\Roaming\Typora\typora-user-images\image-20211208203339089.png)



## 二、常用简单 HTML 标签
| 标签名  | 定义 | 说明 |
| :------- | ------------- | ------------- |
| `<html></html>` | HTML标签  | 页面中最大的标签，我们称之为 根标签 |
| `<head></head>` | 文档头部 | 注意在 head 标签中我们必须要设置的标签是 title |
| `<title></title>` | 文档标题 | 让页面拥有一个属于自己的网页标题 |
| `<body></body>` | 文档主体 | 元素包含页面所有的内容，页面内容基本都是放到 body 里面的 |
| **`<h1> - <h6>`** | **标题** | 加了标题的文字会变粗，独占一行，并且依据重要性h1 - h6递减 |
| **`<p></p>`** | **段落** | 将文档分割为若干段落，段落之间保有空隙，并且会跟随浏览器窗口大小自动换行 |
| **`<br />`** | **换行** | 强制换行，是一个单标签，段落标签会插入一些垂直的间距，而换行标签不会 |
| **`<strong></strong>`**或者`<b></b>` | **加粗** | 加粗字体，更推荐 strong |
| **`<em></em>`**或者`<i></i>` | **倾斜** | 倾斜字体，更推荐 em |
| `<del></del>`或者`<s></s>` | 删除线 | 字体加上删除线，更推荐 del |
| `<ins></ins>`或者`<u></u>` | 下划线 | 字体加上下划线，更推荐 ins |
| **`<div></div>`** | **大盒子** | division，表示分割、分区，一行只能放一个 |
| `<span></span>` | 小盒子 | 表示跨度、跨距，一行上可以放多个 |



## 三、复杂 HTML 标签

### 1. 图像标签

```html
<img src="图像URL" />
```

src 是`<img>`标签的必须属性，它用于指定图像文件的路径和文件名

图像标签的其他属性如下：

![image-20211208211725561](C:\Users\程玉峰\AppData\Roaming\Typora\typora-user-images\image-20211208211725561.png)

src 引入的相对路径和绝对路径要区分：

**相对路径：**图片相对于 HTML 页面的位置

![image-20211208211916430](C:\Users\程玉峰\AppData\Roaming\Typora\typora-user-images\image-20211208211916430.png)

**绝对路径：**是指目录下的绝对位置，直接到达目标位置，通常是从盘符开始的路径

例如：`D:\web\img\logo.gif ` 或完整的网络地址 `http://www.itcast.cn/images/logo.gif`

### 2. 超链接标签

```html
<a href="跳转目标" target="目标窗口的弹出方式"> 文本或图像 </a>
```

两个属性作用如下：

![image-20211208212445875](C:\Users\程玉峰\AppData\Roaming\Typora\typora-user-images\image-20211208212445875.png)

链接分类：

1. 外部链接: 例如 `< a href="http:// www.baidu.com "> 百度</a >`
2. 内部链接:网站内部页面之间的相互链接. 直接链接内部页面名称即可，例如 `< a href="index.html"> 首页 </a >`
3. 空链接: 如果当时没有确定链接目标时，**`< a href="#"> 首页 </a >`**
4. 下载链接: 如果 href 里面地址是一个文件或者压缩包，会下载这个文件
5. 网页元素链接: 在网页中的各种网页元素，如文本、图像、表格、音频、视频等都可以添加超链接
6. 锚点链接: 点击链接,可以快速定位到页面中的某个位置
   - 在链接文本的 href 属性中，设置属性值为<span style="color:red"> #名字 </span>的形式，如`<a href="#two"> 第2集 </a>`
   - 找到目标位置标签，里面添加一个 id 属性 = 刚才的名字 ，如`<h3 id="two"> 第2集 </h3>`

### 3. 表格标签
```html
<table>
 <tr>
     <th>单元格内的文字</th>
 	 <td>单元格内的文字</td>
 ...
 </tr>
 ...
</table>
```

1. `<table> </table>` 是用于定义表格的标签
2. `<tr> </tr>` 标签用于定义表格中的行，必须嵌套在 `<table> </table>`标签中
3. `<th> </th>`标签表示 HTML 表格的表头部分(table head 的缩写)，表头单元格内的文字会加粗居中显示
4. `<td> </td>` 用于定义表格中的单元格(table data 的缩写)，必须嵌套在`<tr></tr>`标签中

表格属性（不常用）如下：

![image-20211211204449813](C:\Users\程玉峰\AppData\Roaming\Typora\typora-user-images\image-20211211204449813.png)

表格结构标签如下：

1. `<thead></thead>`：用于定义表格的头部，`<thead>` 内部必须拥有 `<tr>` 标签， 一般是位于第一行
2. `<tbody></tbody>`：用于定义表格的主体，主要用于放数据本体 
3.  以上标签都是放在 `<table></table>` 标签中

合并表格单元格的方式：

1. 跨行合并：rowspan="合并单元格的个数"
2. 跨列合并：colspan="合并单元格的个数"

![image-20211211204943646](C:\Users\程玉峰\AppData\Roaming\Typora\typora-user-images\image-20211211204943646.png)

### 4. 列表标签

1. 无序列表：以项目符号呈现列表项

   ```html
   <ul>
    <li>列表项1</li>
    <li>列表项2</li>
    <li>列表项3</li>
    ...
   </ul>
   ```

   - 无序列表的各个列表项之间没有顺序级别之分，是并列的
   - `<ul></ul>` 中只能嵌套 `<li></li>`，直接在 `<ul></ul>` 标签中输入其他标签或者文字的做法是不被允许的
   - `<li> 与 </li>` 之间相当于一个容器，可以容纳所有元素

2. 有序列表：列表排序以数字来显示

   ```html
   <ol>
    <li>列表项1</li>
    <li>列表项2</li>
    <li>列表项3</li>
    ...
   </ol>
   ```

   - `<ol></ol>`中只能嵌套`<li></li>`，直接在`<ol></ol>`标签中输入其他标签或者文字的做法是不被允许的
   - `<li> 与 </li>`之间相当于一个容器，可以容纳所有元素

3. 自定义列表：列表项前没有任何项目符号

   ```html
   <dl>
    <dt>名词1</dt>
    <dd>名词1解释1</dd>
    <dd>名词1解释2</dd>
   </dl>
   ```

   - ` <dl></dl>` 里面只能包含` <dt>` 和 `<dd>`
   - `<dt> 和 <dd>`个数没有限制，经常是一个`<dt>` 对应多个`<dd>`

### 5.表单标签

**表单域 **是一个包含表单元素的区域。在 HTML 标签中， `<form>` 标签用于定义表单域，以实现用户信息的收集和传递，`<form>` 会把它范围内的表单元素信息提交给服务器

```html
<form action=“url地址” method=“提交方式” name=“表单域名称">
 各种表单元素控件
</form>
```

常用属性：

![image-20211211210838448](C:\Users\程玉峰\AppData\Roaming\Typora\typora-user-images\image-20211211210838448.png)

表单控件：

1. **input 输入表单元素**：用于收集用户信息

   ```html
   <input type="属性值" />
   ```

   根据不同的 type 属性值，输入字段拥有很多种形式，type 属性值如下：

   ![image-20211211211217471](C:\Users\程玉峰\AppData\Roaming\Typora\typora-user-images\image-20211211211217471.png)

   除 type 属性外，`<input>`标签还有其他很多属性，其常用属性如下：

   ![image-20211211211404090](C:\Users\程玉峰\AppData\Roaming\Typora\typora-user-images\image-20211211211404090.png)

   - name 和value 是每个表单元素都有的属性值,主要给后台人员使用
   - name 表单元素的名字, 要求单选按钮和复选框要有相同的name值
   - checked属性主要针对于单选按钮和复选框, 主要作用一打开页面,就要可以默认选中某个表单元素
   - maxlength 是用户可以在表单元素输入的最大字符数, 一般较少使用

   此外，还有 `<label>`标签用于绑定一个表单元素, 当点击`<label>` 标签内的文本时，浏览器就会自动将焦点(光标)转到或者选择对应的表单元素上,用来增加用户体验

   ```html
   <label for="sex">男</label>
   <input type="radio" name="sex" id="sex" />
   ```

2. **select 下拉表单元素**：定义下拉列表

   ```html
   <select>
    <option>选项1</option>
    <option>选项2</option>
    <option>选项3</option>
    ...
   </select>
   ```

   - `<select>` 中至少包含一对`<option>` 。
   - 在`<option>`中定义 `selected ="selected"` 时，当前项即为默认选中项

3. **textarea 文本域元素**：定义多行文本输入

   ```html
   <textarea rows="3" cols="20">
    文本内容
   </textarea>
   ```

   cols="每行中的字符数"，rows="显示的行数"



## 四、注释方式和特殊字符

### 1. 注释方式
```html
<!-- 注释语句 -->  

快捷方式： ctrl + /
```

### 2.特殊字符

在 HTML 页面中，一些特殊的符号很难或者不方便直接使用，此时我们就可以使用下面的字符来替代

![image-20211208213514300](C:\Users\程玉峰\AppData\Roaming\Typora\typora-user-images\image-20211208213514300.png)

重点记住：<span style="color:red">空格 、大于号、 小于号</span>



## 五、查阅文档

- W3C : http://www.w3school.com.cn/

- MDN：[https://developer.mozilla.org/zh-CN/](https://developer.mozilla.org/zh-CN/)

- 菜鸟教程：[https://www.runoob.com/html/html-tutorial.html](https://www.runoob.com/html/html-tutorial.html)

  
