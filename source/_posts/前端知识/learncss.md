---
title: Hello CSS！
tags: [CSS]
---

概要：
这里主要包括css代码引入位置，选择标签，权值，样式相关，元素分类，以及盒子模型，缩写等知识。



### 1.引入css文件：

内嵌式：
```html
<p style="color:red">这个是段落</p>
```
嵌入式：
```html
<style type="text/css">
span{
	color:red;
}
<style>
```

外部式：写在<head>标签里面
```html
<link href="base.css" rel="stylesheet" type="text/css" />
```

优先级：
内嵌式>嵌入式>外部式,就近原则

<!--more-->

### 2. 选择标签:
标签选择器:

```html
p{
	color:red;
}
```

类选择器:  .
先在html代码厘面写明：
```html
<span class="stress">....</span>
```
然后在css代码里面写明：
```html
.stress{
	color:red;
}
```

外部式：写在<head>标签里面
```html
<link href="base.css" rel="stylesheet" type="text/css" />
```

ID选择器：#
先在html代码厘面写明：
```html
<span id="student">....</span>
```
然后在css代码里面写明：
```html
#student{
	color:red;
}
```

区别:
在于ID选择器在文档里面可以只能使用一次，
可以使用类选择器词列表方法为一个元素同时设置多个样式；
eg:
<span class="stress bigsize">三年级</span>
可以同时将class里面的设置应用于span,而id则不行;


子选择器：> 
作用于第一个后代；
```html
<p class="first">三年级时，<span>我还是一个<span>胆小如鼠</p>
```
css代码选定span：
```
.first>span{
	color:red;
}
```

包含选择器： 空格
用于指定所有后辈；
```
eg：.first span{
	color:red;
}
```

通用选择器：*
匹配html的所有标签；
```
*{
  color:red;	
}
```

伪选择符::
鼠标滑过状态等，要考虑兼容性问题
```
a:hover{
	color:red;
}设置链接划过时显示为红色；
```


分组选择器: ,
eg:
```
h1,span{
	color:red;
}
```
### 3. 继承:
某些样式具有继承性质；
```
p{color:red;}
<p>三年级时，我还是一个<span>胆小如鼠</span>的小女孩。</p>
```
span里面的内容也会显示为红色，继承了p标签；
边框属性不会继承。

### 4. 权值:
多个描述作用于标签时，根据权值高的生效；
标签的权值为1，类选择符的权值为10，ID选择符的权值最高为100。
```
p{color:red;} /*权值为1*/
p span{color:green;} /*权值为1+1=2*/
.warning{color:white;} /*权值为10*/
p span.warning{color:purple;} /*权值为1+1+10=12*/
#footer .note p{color:yellow;} /*权值为100+10+1=111*/
```
继承有权值但是非常低；

层叠：
相同权值下，后面的会覆盖前面的属性；

！important:
强行提高权值，优先级大于用户自己设置的样式；

### 5.样式相关

文字排版字体：
```
body{
	font-family:"Microsoft Yahei";
}
```
字号颜色：
```
font-size:10px;
color:red/#666(灰色);
```

文字样式：
```
font-weight:bold;粗体
```

斜体：
```
font-style:italic;
```

下划线：
```
text-decoration:underline;
```

为原价添加删除线：
```
text-decoration:line-through;
```

设置段首两个空格：
```
p{
	text-indent:2em;
}
```

行间距：
```
p{
	line-height:2em;
}
```

字母间距和单词间距：
```
letter-spacing:50px;
world-spacing:50px;
```
块状文本元素的位置：
```
text-align:center/left/right;
```
### 6. 元素分类：
块状元素：
```
<div>,<p>,<h1>...<h6>,<ol>,<ul>,<dl>,<table>,<address>,<blockquote>,<form>
```
内联元素：
```
<a>,<span>,<br>,<i>,<em>,<strong>,<label>,<q>,<var>,<cite>,<code>
```

内敛块状元素：
```
<img>,<input>
```

块状元素(block)：
内联元素转块级元素：
```
a{display:block;}
```
每个块级元素从新的一行开始，其后元素也另起一行，高度宽度行高和顶底边距都可以设置；
不设置的情况下，是它父元素的100%；


内联元素(inline)：
块级元素转内联元素：
```div{
	display:inline;
}
```
内联元素特点：
和其他元素都在一行；
元素的宽度，高度，顶和底的间距不可以设置；
元素的宽度就是它包含的文字或者图片的宽度，不可改变；


内联块状元素(inline-block):
将元素设置为内联块状元素：
```
display:inline-block;
```
特点：
和其他元素在一行，
元素的宽度，高度，顶和底的间距可以设置；


### 7. 盒子模型：
```
<div>,<p>,<h1>...<h6>,<ol>,<ul>,<dl>,<table>,<address>,<blockquote>,<form>
```
都具有盒子模型的属性。

![cmd-markdown-logo](http://images.cnblogs.com/cnblogs_com/cchyao/%E6%A0%87%E5%87%86W3C%E7%9B%92%E5%AD%90%E6%A8%A1%E5%9E%8B%E5%92%8CIE%E7%9B%92%E5%AD%90%E6%A8%A1%E5%9E%8BCSS%E5%B8%83%E5%B1%80%E7%BB%8F%E5%85%B8%E7%9B%92%E5%AD%90%E6%A8%A1%E5%9E%8B/1.JPG)
边框(border):
粗细，样式，颜色；
```
div{
	border:2px solid red;
}
```
也可以写成
```
div{
	border-width:2px;
	border-style:solid;
	border-color:red;
}
```
border-style:dashed(虚线)|dotted(点线，由点组成的线)|solid(实线)；

选定一个方向的边框线：
```
border-bottom:1px solid red;
其他方向为border-top,border-left,border-right;
```

padding:边框内部距离边框的距离，叫内边距。
```
padding-top,padding-left,padding-right,padding-bottom
```

margin:边框外部距离边框的距离，叫外边距。

### 8. 布局模型：
流动模型，浮动模型，层模型；

**流动模型**：
块状元素的宽度为100%，自上而下按顺序垂直延伸分布；
内联元素都会在所处的包含元素内从左到右水平分布显示。

**浮动模型**：
float：left;

**层模型**：
绝对定位：
```
position:absolute;
```
然后使用left,right,top,bottom相对于父包含块进行绝对定位；
不存在时，相对于浏览器窗口；

相对定位：
```
position:relative;
```
然后使用left,right,top,bottom相对于正常的位置进行相对移动；

固定定位：
```
position:fixed;
```
相对于浏览器视图位置不变，拖动滚动条位置不变。

**子元素定位到父元素的某个位置：**
父元素：
```
position:relative;
```
子元素:
```
position:absolute;
top:10px；
left:20px;
```

盒模型margin,padding,border按顺时针设置:
上右下右；
可以简写:
```
如margin:10px 20px 10px 20px;
写成margin：10px 20px;
和margin:10px 20px 30px 20px;
简写成margin:10px 20px 30px;
```
颜色值的缩写：
每两位相同，可以缩写一半；
```
p{
	color:#336699;=== color=#369;
}
```
字体缩写：
完整的设置：
```
body{
    font-style:italic;
    font-variant:small-caps; 
    font-weight:bold; 
    font-size:12px; 
    line-height:1.5em; 
    font-family:"宋体",sans-serif;
}
```
常见缩写：
至少设置font-size和font-family属性，注意font-size和line-height要加斜杠；
```
body{
    font:12px/1.5em  "宋体",sans-serif;
}
```

** 9. 颜色值 **

1.英文颜色：

```
p{
color:red;
}
```

2.RGB颜色：
每个数值在0-255之间，也可以用百分比表示；
```
p{
color:rgb(133,20,210);
}
```

3.十六进制颜色：
将RGB的值直接写成16进制；
```
p{
color:#00ff00;
}
```

**长度值**：
px:像素；
em:倍数；
百分比:%;



### 10. 水平居中设置：

**行内（内联）元素：**
如果是文本，图片等行内元素，水平居中通过给父元素设置text-align:center;

**定宽块状元素：**

块状元素宽度为固定值：
div{
	width:200px;
	margin:20px auto;
}

**不定宽块状元素：**
第一种：
加入table标签；
table长度根据其内文本程度决定，实际是定宽的块元素，可用过margin:20px auto来设置。
```
<div>
    <table>
        <tr>
            <td>
                <div class="wrap">设置我所在的div容器水平居中</div>
            </td>
            
        </tr>
    </table>
</div>
```

第二种：
改变块级元素为行内元素：
display:inline;
然后使用text-align:center;实现居中效果。
html代码：
```
<body>
<div class="container">
    <ul>
        <li><a href="#">1</a></li>
        <li><a href="#">2</a></li>
        <li><a href="#">3</a></li>
    </ul>
</div>
</body>
```
css代码：

```
<style>
.container{
    text-align:center;
}
/* margin:0;padding:0（消除文本与div边框之间的间隙）*/
.container ul{
    list-style:none;
    margin:0;
    padding:0;
    display:inline;
}
/* margin-right:8px（设置li文本之间的间隔）*/
.container li{
    margin-right:8px;
    display:inline;
}
</style>
```

方法三：
这个方法很有意思：
父元素：
clear:both;清除所有浮动；
position:relative;
float:left;
left:50%;

这个时候父元素的位置在中线处；

子元素：
position:relative;
float:left;
left:-50%;
子元素相对于父元素向左移动一半，那么子元素的中线与屏幕中线吻合；

代码示例如下：

```
.wrap{
    clear:both;
    float:left;
    position:relative;
    left:50%;
    
}
.wrap-center{
    background:#ccc;
    float:left;
    position:relative;
    left:-50%;
    
}
```

html代码：

```
<div class="wrap">
    <div class="wrap-center">我们来学习一下这种方法。</div>
</div>
```

### 11.垂直居中设置：###

**父元素高度确定的单行文本：**
设置height与line-height高度一致来实现；

**父元素高度确定的多行文本**
方法一：
使用插入 table  (包括tbody、tr、td)标签，同时设置td标签为 vertical-align：middle。
实例代码：
html：
```
<table>
    <tbody>
        <tr>
            <td class="wrap1">
               <div>
    <img src="http://img.mukewang.com/54ffac56000169c001840181.jpg" title="害羞的小女生"/>
</div> 
            </td>
        </tr>
    </tbody>
</table>
```
css：

```
.wrap1{height:300px;background:#ccc;vertical-align:middle;}
```
 
 方法二：
 设置块级元素为display:table-cell;vertical-align:middle;
 但是缺点也是很明显的，不兼容IE6、7，也修改了块状元素的性质。
 html:
```
<div class="container1">
    <img src="http://img.mukewang.com/54ffac56000169c001840181.jpg" title="害羞的小女生"/>
</div>
```
css:
```
.container1{
    height:300px;
    border:1px solid red;
    display:table-cell;
    vertical-align:middle;
    padding:0px;
    margin:0px;
}
```

**改变display类型**
使用
```
position:absolute
float:left/right;
```
可以变为
```
display:inline-block
```
可以设置宽和高了；
