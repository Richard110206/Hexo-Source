---
title: HTML Fundamentals Guide
date: 2025-08-12 17:37:03
tags: [html]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/HTML.png?raw=true
category: Full Stack
category_bar: true
description: A brief tutorial on HTML, including the basic grammar, detailed example and demonstration!
---



{%note default%}
或许我现在才深刻认识到计算机是一门实践的学科还不算太晚，上学期我抱着书将HTML和CSS啃完，看似语法从“入门”到“精通”，但没有项目进行实战，导致记忆并不深刻，仅仅“**纸上得来终觉浅**” ，遂现在以一个案例进行引入，学习（亦或是复习）HTML和CSS的语法！{%endnote%}

进阶速通版本参看 [Web-Dev-Beginner - Quick start to front & back-end Dev](https://doc.duke486.com/)

## What is HTML？

HTML(Hyper Text Markup Language)是**超文本编辑语言**，与我们平时学习的c++、python等编程语言，它是一种标记语言，可以理解为markdown语法的进阶，毕竟我们博客撰写的markdown最后都是通过渲染为HTML展示在用户面前的，其由一套标记标签组成，大多数标签**由一对尖括号包裹**，他们定义了网页中每个元素的作用和显示方式，像是这样：`<标签名>文本内容</标签名>`，这种形式的叫做**双标签**，标签内的内容会被浏览器渲染。比如：

```html
<p>这是一个段落</p>
```

有些标签是单独存在的，不需要结束标签，这些叫做**自闭和标签**，例如：

```html
<img sre="image.jpg" alt="描述">
```
## HTML basic structure
在vscode中输入`!`，在按下`Tab`，会自动补全HTML的基本结构：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```
1. 开始标签`<html>`：限定了文档的开始和结束点
2. 头部标签`<head>`：描述文档的各种属性和信息
3. 标题标签`<title>`：定义文档标题，通常会直接显示在浏览器窗口的标题栏或状态栏上，当用户将网页收藏或作为书签时，标题将成为文档链接的默认名称
4. 主题标签`<body>`：包含文档所有内容，后续我们主要的HTML代码也在这里编写
5. 元信息标签`<meta>`：提供有关页面的信息，其永远位于head元素内部
6. `<!DOCTYPE>`标签:必须位于HTML文档第一行，没有结束标签，且不区分大小写

## HTML基本语法
### 标题文字
```html
    <h1>一级标题</h1>
    <h2>二级标题</h2>
    <h3>三级标题</h3>
    <h4>四级标题</h4>
    <h5>五级标题</h5>
    <h6>六级标题</h6>
```
<h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题</h6>
{%note primary%}
看上去与markdown中的`#`效果差不多{%endnote%}

{%note danger%}
不要为了使文字加粗而使用h标签，文字加粗使用b标签
这里标题会在目录形成**树形结构**！{%endnote%}

### 文字对齐

网页中都是使用**默认的对齐方式**，但当我们需要其他的对齐方式时就需要使用`align`属性进行设置

|属性值|含义|
|-----|-------|
left|左对齐（默认）|
center|居中对齐|
right|右对齐|
```html
align="对齐方式"
```
```html
<h1 align="center">这是一个居中的一级标题</h1>
```
<h1 align="center">这是一个居中的一级标题</h1>

### 文字样式
在HTML中，字体效果必须在浏览器安装相应字体后才能浏览，否则还是会被浏览器中的通用字体所替代。
| 字体名称           | 适用场景                |
|--------------------|------------------------|
| **微软雅黑**       | 中文网页/UI设计首选字体 |
| **黑体**           | 中文标题/广告宣传       |
| **宋体**           | 中文印刷/正式文档       |
| **楷体**           | 中文手写风格/艺术排版   |
| **仿宋**           | 中文公文/古籍排版       |
| **Georgia**        | 标题、优雅排版          |
| **Times New Roman**|  印刷品、正式文档        |
```html
<font face="Simsun">应用了宋体文字</font>
```
<font face="Simsun">应用了宋体文字</font>

### 段落换行
```html
<br>此处换行
```

```html
<p>It's been a long day without you, my friend.And I'll tell you all about it when I see you again.We've come a long way from where we began.Oh I'll tell you all about it when I see you again</p>


<p>It's been a long day without you, my friend.<br>And I'll tell you all about it when I see you again.<br>We've come a long way from where we began.<br>Oh I'll tell you all about it when I see you again</p>
```

<p>It's been a long day without you, my friend.And I'll tell you all about it when I see you again.We've come a long way from where we began.Oh I'll tell you all about it when I see you again.</p>



<p>It's been a long day without you, my friend.<br>And I'll tell you all about it when I see you again.<br>We've come a long way from where we began.<br>Oh I'll tell you all about it when I see you again.</p>

——《See You Again》



### 字体颜色
```html
<font color="颜色参数"></font>
```
颜色参数值有多种表达形式：
#### 1. 颜色关键字
直接使用预定义的颜色名称（不区分大小写）：
```html
<p style="color: red;">红色</p>
<p style="color: blue;">蓝色</p>
<p style="color: green;">绿色</p>
<p style="color: orange;">橙色</p>
<p style="color: purple;">紫色</p>
<p style="color: gray;">灰色</p>
<p style="color: black;">黑色</p>
```
<p style="color: red;">红色</p>
<p style="color: blue;">蓝色</p>
<p style="color: green;">绿色</p>
<p style="color: orange;">橙色</p>
<p style="color: purple;">紫色</p>
<p style="color: gray;">灰色</p>
<p style="color: black;">黑色</p>


#### 2. 十六进制（HEX）
格式：#RRGGBB 或 #RGB（简写）
```html
<p style="color: #FF0000;">红色（完整）</p>
<p style="color: #00FF00;">绿色（完整）</p>
<p style="color: #0000FF;">蓝色（完整）</p>
<p style="color: #F00;">红色（简写）</p>
<p style="color: #0F0;">绿色（简写）</p>
<p style="color: #00F;">蓝色（简写）</p>
<p style="color: #FFA500;">橙色</p>
<p style="color: #800080;">紫色</p>
```
<p style="color: #FF0000;">红色（完整）</p>
<p style="color: #00FF00;">绿色（完整）</p>
<p style="color: #0000FF;">蓝色（完整）</p>
<p style="color: #F00;">红色（简写）</p>
<p style="color: #0F0;">绿色（简写）</p>
<p style="color: #00F;">蓝色（简写）</p>
<p style="color: #FFA500;">橙色</p>
<p style="color: #800080;">紫色</p>

#### 3. RGB/RGBA
格式：rgb(R, G, B) 或 rgba(R, G, B, A)（带透明度）
```html
<p style="color: rgb(255, 0, 0);">红色</p>
<p style="color: rgb(0, 255, 0);">绿色</p>
<p style="color: rgb(0, 0, 255);">蓝色</p>
<p style="color: rgba(255, 0, 0, 0.5);">半透明红色</p>
<p style="color: rgba(0, 255, 0, 0.3);">30%透明绿色</p>
<p style="color: rgba(0, 0, 255, 0.7);">70%透明蓝色</p>
<p style="color: rgb(255, 165, 0);">橙色</p>
<p style="color: rgba(128, 0, 128, 0.6);">60%透明紫色</p>
```
<p style="color: rgb(255, 0, 0);">红色</p>
<p style="color: rgb(0, 255, 0);">绿色</p>
<p style="color: rgb(0, 0, 255);">蓝色</p>
<p style="color: rgba(255, 0, 0, 0.5);">半透明红色</p>
<p style="color: rgba(0, 255, 0, 0.3);">30%透明绿色</p>
<p style="color: rgba(0, 0, 255, 0.7);">70%透明蓝色</p>
<p style="color: rgb(255, 165, 0);">橙色</p>
<p style="color: rgba(128, 0, 128, 0.6);">60%透明紫色</p>

### 文字的上下标
```html
<sup></sup>上标标签
<sub></sub>下标标签
```
```html
x<sup>3</sup>+x<sup>2</sup>+x+1=0
x<sub>3</sub>+x<sub>2</sub>+x+1=0
```
x<sup>3</sup>+x<sup>2</sup>+x+1=0
x<sub>3</sub>+x<sub>2</sub>+x+1=0

### 文字的删除线
```html
<strike>删除的文字</strike>
```
<strike>删除的文字</strike>

### 文字加粗
```html
<br>需要加粗的文字</br>
```
**需要加粗的文字**

### 给网页添加图片
#### 图片格式
网页中的图像格式通常有三种，即GIF、JPEG、PNG，目前前两者的支持情况最佳，多数浏览器都可以兼容，而PNG格式的图片属于无损压缩，其清晰度更高，且支持图片保留透明度，因而其所占存储空间对比GIF和JPEG稍大。

```html
<img src="图片文件地址">
```
src可以是绝对路径，也可以是相对路径，也可以是图片的网络链接地址
```html
<img src="/banner_img/background.jpg">
```
<img src="/banner_img/background.jpg">

#### 图片大小
```html
<img src="图片文件地址" width="图片的宽度" height="图片的高度">
```
```
<img src="/banner_img/background.jpg" width="500px" height="300px">
```
<img src="/banner_img/background.jpg" width="500px" height="300px">

#### 水平间距
如果不进行换行，那么添加得到图像会紧跟在文字后面，也可以用来设置图片间间距效果
```html
<img src="图片文件地址" hspace="水平间距">
```
```html
<img src="/banner_img/background.jpg" width="70px" height="40px" hspace="30px">
<img src="/banner_img/background.jpg" width="70px" height="40px" hspace="30px">
<img src="/banner_img/background.jpg" width="70px" height="40px" hspace="30px">
```

<img src="/banner_img/background.jpg" width="100px" height="60px" hspace="30px">
<img src="/banner_img/background.jpg" width="100px" height="60px" hspace="30px">
<img src="/banner_img/background.jpg" width="100px" height="60px" hspace="30px">

#### 提示文字
当指针放在图片上面时会有提示文字
```html
<img src="图片文件地址" title="提示文字">
```
```html
<img src="/banner_img/background.jpg" title="background">
```
<img src="/banner_img/background.jpg" title="background">

### 创建表格
|标记|含义|
|-----|-------|
`<table></table>`|表格标记|
`<tr></tr>`|行标记|
`<td></td>`|单元格标记|
```html
<table>
<tr>
    <th>姓名</th>
    <th>学号</th>
    <th>成绩</th>
</tr>
<tr>
    <td>张三</td>
    <td>123456</td>
    <td>90</td>
</tr>
<tr>
    <td>李四</td>
    <td>654321</td>
    <td>85</td>
</tr>
</table>
```
`<th></th>`为表格文字加粗

<table>
<tr>
    <th>姓名</th>
    <th>学号</th>
    <th>成绩</th>
</tr>
<tr>
    <td>张三</td>
    <td>08241110</td>
    <td>90</td>
</tr>
<tr>
    <td>李四</td>
    <td>08241120</td>
    <td>95</td>
</tr>
</table>

### 行的的背景
```html
<tr bgcolor="颜色值"><tr>
```

### 表格的对齐方式
#### 水平对齐
```html
<tr align="水平对齐方式"><tr>
```
#### 垂直对齐
```html
<tr valign="垂直对齐方式"><tr>
```
### 表格的的背景
#### 背景颜色
```html
<tr bgcolor="颜色值"><tr>
```
#### 背景图片
```html
<table background="图片地址"></table>
```

### 单元格格式
```html
<td width="单元格宽度" height="单元格高度">单元格内容</td>
```
### 合并单元格
- colspan合并的是行相邻的单元格
- rowspan合并的是列相邻的单元格

```html
<table>
<tr>
<th>星期一</th>
<th>星期二</th>
<th>星期四</th>
<th>星期五</th>
</tr>
<tr>
    <td rowspan="2">语文</td>
    <td>数学</td>
    <td>英语</td>
    <td>物理</td>
</tr>
<tr>
    <td>物理</td>
    <td>化学</td>
    <td>生物</td>
</tr>
<tr>
<td colspan="4" align="center">课间活动</td>
</tr>
<tr>
    <td>物理</td>
    <td>化学</td>
    <td>生物</td>
    <td>政治</td>
</tr>
<tr>
    <td>政治</td>
    <td>历史</td>
    <td>地理</td>
    <td>生物</td>
</tr>
</table>
```

<table>
<tr>
<th>星期一</th>
<th>星期二</th>
<th>星期四</th>
<th>星期五</th>
</tr>
<tr>
    <td rowspan="2">语文</td>
    <td>数学</td>
    <td>英语</td>
    <td>物理</td>
</tr>
<tr>
    <td>物理</td>
    <td>化学</td>
    <td>生物</td>
</tr>
<tr>
<td colspan="4" align="center">课间活动</td>
</tr>
<tr>
    <td>物理</td>
    <td>化学</td>
    <td>生物</td>
    <td>政治</td>
</tr>
<tr>
    <td>政治</td>
    <td>历史</td>
    <td>地理</td>
    <td>生物</td>
</tr>
</table>

### 无序列表
```html
<ul>
<li>列表项目1</li>
<li>列表项目2</li>
<li>列表项目3</li>
</ul>
```
<ul>
<li>列表项目1</li>
<li>列表项目2</li>
<li>列表项目3</li>
</ul>

```html
<ul type="符号类型"></ul>
```

|参数值|含义|
|-----|-----|
|disc|实心圆形|
|circle|空心圆形|
|square|实心方形|

```html
<ul type="circle">
    <li>无序列表</li>
    <li>有序列表</li>
    <li>定义列表</li>
</ul>
```

<ul type="circle">
    <li>无序列表</li>
    <li>有序列表</li>
    <li>定义列表</li>
</ul>

同时也可以给每一个无序列表的选项进行符号类型的选择：

```html
<ul>    
    <li type="disc">实心列表</li>
    <li type="circle">空心列表</li>
    <li type="square">方形列表</li>
</ul>
```

<ul>    
    <li type="disc">实心列表</li>
    <li type="circle">空心列表</li>
    <li type="square">方形列表</li>
</ul>

### 有序列表
```html
<ol>
<li>列表项目1</li>
<li>列表项目2</li>
<li>列表项目3</li>
</ol>
```
<ol>
<li>列表项目1</li>
<li>列表项目2</li>
<li>列表项目3</li>
</ol>

也可以自定义起始值和序号类型
```html
<ol start=起始数值 type="序号类型"></ol>
```
{%note info %}
无论数字还是字母等类型，起始值只能是数字！
{%endnote %}

### 创建超链接

#### 内部链接

```html
<a href="链接地址" target="目标窗口的打开方式">链接文字</a>
```
| `target`属性值 | 打开方式 |
|----------|-----------|
|`_self`|在当前窗口打开|
|`_blank`|在新窗口打开|
|`_parent`|在父窗口打开|
|`_top`|在顶层窗口打开|


#### 外部链接

```html
<a href="http://...">...</a>
```
至此，你已经学会了有关静态网页HTML的语法，可以尝试使用HTML完成个人博客的简单撰写！

## 表单
HTML 表单用于收集用户的输入信息，当我们需要与用户进行交互，制作动态网页时，就需要使用到表单！
```html
<form name="test" action="mail:08241120@cumt.edu.cn" method="post" target="_blank">
</form>
```
{%note info %}
- 处理动作`action`：定义表单要提交的地址，也就是表单中收集到的资料将要传递的程序地址
  
  - 绝对地址
  - 相对地址
  - E-mail地址等
- 表单名称`name`：定义表单的名称，这个名称将作为表单的标识符，用于提交表单时使用
- 传送方式`method`：定义表单中数据的传送方式

  - `get`：用户端直接发送给服务器，速度快，但数据长度不能太长
  - `post`：用户端计算机通知服务器来读取数据，数据长度没有限制，但速度较慢
- 目标显示方式`target`：定义表单提交后，结果显示在哪个窗口中（相关参数见上文）

{%endnote %}

### `input`标签
```html
<form>
    <input name="空间名称" type="控件类型"/>
</form>
```
|`type`取值 |含义|
|----|----|
|`text`|文本字段|
|`password`|密码字段|
|`radio`|单选按钮|
|`checkbox`|复选框|
|`button`|普通按钮|
|`submit`|提交按钮|
|`reset`|重置按钮|

#### 文字字段`text`
```html
     <input name="控件名称" type="text" value="字段默认值" size="控件长度" maxlength="最长字符数">
```
{%note warning %}
- `name`：文字字段名称，用来与其他控件进行区分
- `size`：文本框的显示长度
- `maxlength`：文本框中最多可输入的文字数
- `value`：文本框中的默认值
{%endnote%}

<form>
 <input name="test" type="text" value="请输入你的想法" size="15" maxlength="100">
</form>

```html
<form>
 <input name="test" type="text" value="请输入你的想法" size="15" maxlength="100">
</form>
```

#### 密码域`password`

输入到密码域的文字内容都以“*”或者圆点显示。
```html
     <input name="控件名称" type="password" value="字段默认值" size="控件长度" maxlength="最长字符数">
```

<form>
 <input name="test" type="password" value="请输入你的想法" size="15" maxlength="100">
</form>

```html
<form>
 <input name="test" type="password" value="请输入你的想法" size="15" maxlength="100">
</form>
```
虽然密码域的输入字符已经以掩码的形式显示了，但是没有做到**真正的保密**，因为用户可以通过复制改密码没中的内容并粘贴到其它文档中，查看到密码的“真面目”。为了实现密码的真正安全，可以将密码域的**复制功能屏蔽**，同时**改变密码域的掩码字符**！

<form>
  <input name="test" type="password" value="请输入你的想法" size="15" maxlength="100" oncopy="return false" oncut="return false" onpaste="return false">
</form>

```html
<form>
  <input name="test" type="password" value="请输入你的想法" size="15" maxlength="100" oncopy="return false" oncut="return false" onpaste="return false">
</form>
```

#### 复选框`checkbox`
```html
     <input name="复选框名称" type="checkbox" value="复选框的值" checked="checked/>
```
<form>
  <input type="checkbox" name="colors" value="red" checked> Red<br>
  <input type="checkbox" name="colors" value="green"> Green<br>
  <input type="checkbox" name="colors" value="blue"> Blue<br>
</form>
确保所有相关复选框有相同的name属性，这样才能将选中的复选框值一起提交。
```html
<form>
  <input type="checkbox" name="colors" value="red" checked> Red<br>
  <input type="checkbox" name="colors" value="green"> Green<br>
  <input type="checkbox" name="colors" value="blue"> Blue<br>
</form>
```


#### 表单按钮`button`
```html
<input name="按钮名称" type="button" value="按钮的值" onclick="处理程序">
```

<form>
  <input name="按钮名称" type="button" value="点击试试" onclick="window.close()">
</form>

```html
<form>
  <input name="按钮名称" type="button" value="点击试试" onclick="window.close()">
</form>
```

#### 提交按钮`submit`
```html
<input name="按钮名称" type="submit" value="按钮的值">
```
<form>
<input name="按钮名称" type="submit" value="提交">
</form>
```html
<form>
<input name="按钮名称" type="submit" value="提交">
</form>
```

#### 重置按钮`reset`
用来清楚用户在页面上输入的信息
```html
<input name="按钮名称" type="reset" value="重置">
```

<form>
  <input name="按钮名称" type="reset" value="重置">
</form>

```html
<form>
  <input name="按钮名称" type="reset" value="重置">
</form>
```
#### 文件域`file`
用于在表单中添加图片或者文件
```html
<input name="名称" type="file" size="控件长度" maxlength="最长字符数">
```

<form>
<input name="名称" type="file" size="控件长度" maxlength="最长字符数">
</form>

```html
<form>
<input name="名称" type="file" size="控件长度" maxlength="最长字符数">
</form>
```

#### 文本域`textarea`
```html
<textarea name="名称"cols="列数" rows="行数" value="文本默认值">文本内容</textarea>
```

<form>
<textarea name="名称"cols="40" rows="3">文本内容</textarea>
</form>

```html
<form>
<textarea name="名称"cols="40" rows="3">文本内容</textarea>
</form>
```
### `label`表单定义标签
{%note info %}
- 显式关联：将文本和表单控件一起放在`label`标签内
- 隐式关联：将`label`标签的`for`属性与表单控件的`id`属性关联起来
{%endnote%}

<form>
    <label for="male">姓名</label>
    <input type="checkbox" name="sex" id="male"/>
    <br>
    <label for="female">密码</label>
    <input type="radio" name="sex" id="female"/>
</form>


```html
<form>
    <label for="male">姓名</label>
    <input type="checkbox" name="sex" id="male"/>
    <br>
    <label for="female">密码</label>
    <input type="radio" name="sex" id="female"/>
</form>
```

这样我们就将**文本内容与控件关联起来**，当点击表单控件前文字时，该表单控件就可以被选中。

#### 列表表单
```html
<select mutiple size="可见选项数">
<option value="值" selected="seclected"></option>
</select>
```

<form>
  <select name="list1">
    <option value="美食小吃">美食小吃</option>
    <option value="火锅">火锅</option>
    <option value="麻辣烫">麻辣烫</option>
    <option value="砂锅">砂锅</option>
 </select>
 <select name="list1" mutiple size="4">
    <option value="美食小吃">美食小吃</option>
    <option value="火锅">火锅</option>
    <option value="麻辣烫">麻辣烫</option>
    <option value="砂锅">砂锅</option>
</select>
</form>

```html
<form>
  <select name="list1">
    <option value="美食小吃">美食小吃</option>
    <option value="火锅">火锅</option>
    <option value="麻辣烫">麻辣烫</option>
    <option value="砂锅">砂锅</option>
 </select>
 <select name="list1" mutiple size="4">
    <option value="美食小吃">美食小吃</option>
    <option value="火锅">火锅</option>
    <option value="麻辣烫">麻辣烫</option>
    <option value="砂锅">砂锅</option>
</select>
</form>
```

封面来源：[HTML & CSS for Beginners | FREE MEGA COURSE (7+ Hours!)](https://www.youtube.com/watch?v=iG2jotQo9NI&t=2120s)