## CSS - cascading style sheet

## 基本构成

1. 样式表
2. 规则
3. 选择器+声明块
4. 声明
5. CSS属性+CSS属性值组成的键值对

```css
p{
 color:red;
 font-size:13px;
}
```

```css
div ul li #test{
    
}
```

:warning:选择器并无优先级，优先级是针对声明的！

## 大纲

* 选择器
* 自定义字体
* 新的UI方案
* 过渡
* 2d/3d变形
* 动画
* 布局拓展
* 变量和函数

## 基本选择器

基本选择器：

* id `#{}`
* class类选择器 `.{}`
* 元素选择器 `p{}`
* 通配符选择器/后代选择器 `p span{}`
* 分组选择器 `.rectangle,.circle{}`

:warning: id选择器已经有了唯一性，不推荐与其他选择器混用

## 复合选择器

* 交集选择器：选择满足多个选择器条件对应的元素： **and**

> 交集选择器中如果有元素选择器，必须使用元素选择器开头

```css
.usa.china{color:red}
```

* 并集选择器（选择器分组）：同时选择多个选择器对应的元素 : **or**

```css
h1,span{color:green;}
```

:tada:  不同类型的选择器可以使用以上规则混用，无限组合

## 关系选择器

### 说明

* 父元素：**直接**包含子元素的元素叫做父元素
* 子元素：**直接**被父元素包含的元素叫做父元素
* 祖先元素：**直接或间接**包含后代元素的元素叫做祖先元素；一个元素的父元素也是祖先元素
* 后代元素：**直接或间接**被祖先元素包含的元素叫做后代元素；子元素也是后代元素
* 兄弟元素：拥有相同父元素的元素是兄弟元素

### 语法

子元素选择器： `.box>p{}` 选择指定父元素的指定子元素

后代元素选择器： `.box p{} ` 选择指定元素内的指定后代元素

相邻兄弟选择器： `.center + .neighbour{}` 选择下一个兄弟

> 目标的兄弟是文档流中下一个紧跟的元素，会被其他元素阻断

通用兄弟选择器：`.center ~ .neighbour{}` 选择下面所有的兄弟

> 目标的兄弟是文档中下几个同级的元素，不会被其他元素阻断

:flags:  ​学习定义的时候需要注意

* 默认值
* 属性是否可以继承

## 属性选择器

`p[nickname]` 选择含有唯一指定属性名的元素

`p[nickname="pony"]` 选择含有**唯一**指定属性值的元素

> Boolean Number等其他类型同理

`p[nickname~="pony"] `选择包含有指定属性值的元素

> 当属性值有多个时，使用此语法块定义的声明生效

模糊匹配：

`[属性名^=属性值]` 指定属性值开头的元素

`[属性名$=属性值]` 指定属性值结束的元素

`[属性名*=属性值]` 属性值中含有某值的元素

## 伪类 :

### 关系伪类

伪类即不存在的类，用来描述一个元素的**特殊状态**，比如第一个子元素，被点击、聚焦、悬浮的元素。

> 可以把`:XXX`看做是一个`.XXX`类，`p:hover`中的p和hover是and关系

在CSS3+标准的语法中，伪类统一使用冒号开头，具体有：

* `:first-child` 是父元素的第一个子元素
* `:last-child`是父元素的最后一个子元素
* `:nth-child(n)`是父元素的指定子元素

> n为确定的数字（0-正无穷），如果写n代表全部选中，2n/even代表偶数，2n-1/odd代表奇数

`:first-of-type` 是父元素的第一个指定类型的子元素

`:last-of-type` 是父元素的最后一个指定类型子元素

`:nth-of-type ` 是父元素自定义位置的指定类型子元素



`:not` 否定伪类，将符合条件的元素从选择器中去除

> not嵌套其他选择器使用

### 事件伪类

`a:link` 正常的链接（默认）

`a:visited` 访问过的链接

> 由于隐私原因，:visited只能修改颜色

`div:hover` 鼠标悬浮在元素上方时

`div:active`鼠标正在点击元素时

`input:focus`鼠标聚焦元素时

## 伪元素 ::

伪元素是一个附加至选择器末的关键词 `::`，允许你对被选择元素的**特定部分**修改样式。

> 因为样式是需要应用于元素的，伪元素相当于为特定部分包裹标签，然后单独修改其样式

`::first-letter`第一个字母

`::first-line`第一行

`::selection` 光标选中的内容



`::before`指定元素的所有内容之前的位置

`::after`指定元素的所有内容之后的位置

> 相当于元素innerHTML之前

使用`::before`和​`::after`必须结合`content`属性来使用

此方法生成的元素无法被选中。

:car:例子：

```html
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
	<style>
		div::before {
			content: '我想看到:';
		}
		div::after {
			content: '就是这样';
			color: red;
		}
	</style>
</head>

<body>
	<div>
		Hello Javascript!
	</div>
</body>
</html>
```

具体效果：

![image-20210505195401593](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210505195401593.png)

> 例如html5的q标签会自动在开头和结尾使用伪元素语法加上`""`



:pencil2:CSS选择器餐厅练习 [CSS Diner - Where we feast on CSS Selectors! (flukeout.github.io)](https://flukeout.github.io/)