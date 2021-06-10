## CSS - cascading style sheet

[toc]

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

`a:target`

> 由于隐私原因，:visited只能修改颜色

> link+visited > hover +  active 

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

## 样式继承

我们为一个元素设置的样式也会应用到他的后代元素。继承是发生在**祖先**和**后代元素**之间的。

继承的设计是为了方便我们的开发，利用继承我们可以将一些通用的样式统一设置到我们共同的祖先元素上，这样我们只需设置一次即可让我们所有的元素都具有该样式。

> 背景相关的、布局相关的等不会被继承

在MDN Web Docs可以查询一个样式是否能被继承

## 选择器的权重

:bomb:样式冲突：当我们通过不同的选择器，选择相同的元素，并且为相同的样式设置不同的值时，此时就发生了冲突

发生样式冲突时，应该用哪个样式由选择器的权重决定

选择器的权重：(由高到低)

* 内联  1,0,0,0 
*  id选择器  1,0,0
*  类和伪类选择器  1,0 
* 元素选择器   1 
* 通配选择器  0
* 继承的样式 NaN

![image-20210509164616264](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210509164616264.png)

交集：比较优先级时，需要将所有选择器的优先级进行相加计算，最后优先级越高，则越优先显示

并集：分组选择器分别进行计算，取最大的权重

:bulb:选择器的累加不可能超过其数量级

>  11个**id选择器**交集也不会超过**内联**样式的权重

:black_flag: 选择器越具体，优先级越高

:flags:如果选择器的优先级相等，优先使用新出现的

:warning:给样式后面加入`!important`关键字可以将优先级设置为最高（超过内联）；慎用

## 像素和百分比

像素：

* 屏幕（显示器）实际上是由一个一个发光的小点组成的

* 不同屏幕的像素观感大小是不同的，同样尺寸的屏幕像素点越小整体观感越清晰

百分比：

* 可以将属性值设置为相对其父元素属性的百分比值
* 设置百分比可以使子元素跟随父元素的改变而改变

em:

* 相对于**当前元素**的字体大小来计算
* 1em = 1 font size (宽×高)； 10 em = 10 font size
* 跟随字体大小的改变而改变

> font-size的具体大小根据字体和语言的不同而变化，宽高不一定和给定的一致

例子：以下三种类所描绘的div大小一致

```css
	    .box1 {
			width: 200px;
			height: 200px;
			background-color: orange;
		}

		.box2 {
			width: 50%;
			height: 50%;
			background-color: palevioletred;
		}

		.box3 {
			font-size: 20px;
			width: 10em;
			height: 10em;
			background-color: lightblue;
		}
```

> rem是相对于**根元素**的字体大小即`html{}`来计算的，html的font-size一般默认为16px

## 颜色单位

在CSS中可以直接使用颜色名来设置各种颜色，但是在CSS中直接使用颜色名十分不方便

实际中我们主要使用RBG值来表示（注意是光的三原色：红绿蓝）

RGB就是通过三种不同颜色的浓度来调配出不同的颜色。

每一种颜色的范围在 0 - 255之间（0 - 100% ） 之间

RGB：

* `rgb(0,0,0)` => 黑色

* `rgb(255,255,255)`=>白色

* `rgb(255, 0, 0)`=>红色

* `rgb(0, 255, 0)`=>绿色

* `rgb(0,0,255)`=>蓝色


:bulb:RGBA就是在RBG的基础上增加了一个Alpha通道表示不透明度（范围0.0-1.0，1=100%完全不透明）

十六进制的RGB值：

* 语法：`#红色绿色蓝色`
* 范围：00  -  FF
* 白色：`#FFFFFF`
* 黑色：`#000000` 

> 如果两位两位重复则可以进行简写，如 #BFA - > #BBFFAA

*HSL值、HSLA值：（色相、饱和度、亮度、不透明度）

* Hue (0-360)
* Saturation (0%-100%)
* Lightness  (0%-100%)
* Alpha(0-1)

## 文档流 normal flow

网页是一个多层的结构，一层摞着一层（z-index)，通过CSS可以分别为每一层来设置样式，作为用户来讲只能够看到最上面的一层，这些层中，最底层的一层就叫做**文档流**，文档流是网页的基础

我们所创建的元素默认都是在文档流中进行排列的

:heart:对于我们来说，元素主要有两个状态，一个是在文档流中，一个不在文档流中（脱离文档流）



元素在文档流时的特点：

* 块元素：

  * 默认宽度是父元素的100%，充满一行
  * 默认高度被内容撑开（子元素）
  * 自上向下垂直排列

* 行内元素：

  * 默认宽度和高度总是被其内容所撑开

  * 行内元素不会独占页面一行，只占自身的大小

  * 行内元素在页面中自左向右水平排列

  * 如果一行之中不能容纳下所有的行内元素，则元素会换到第二行继续自左向右排列

    

## 盒模型 box model

CSS 将页面中的所有元素都设置成了矩形的盒子

将元素物化为矩形的盒子后，对页面的布局就变成了将不同的盒子摆放到不同的位置

每一个盒子都由以下几个部分组成

* 内容区 content （ 文字、子元素等）
* 边框 border 
* 内边距 padding
* 外边距 margin

![](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210509183939177.png)

对一个元素设置的`width`和`height`属性值指定了内容区(content)的大小

### 边框 border

指定border需要颜色(border-color)厚度（border-width)和样式（border-style)

> 注意，在默认情况下如果设置了border宽度为10px而其他不变，则盒子宽高各增加10px，如果给定width:200px;height:200px，则盒子观感大小为宽220px*高220px

![image-20210509184838864](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210509184838864.png)

> border-width的默认值一般为3个像素

border-width用于指定四个方向边框的宽度（顺序为 上 右 下 左）

> 简写则为 ：上下左右；上下 左右 ；上  左右 下；

>  `border-width`也可以拆分为 `border-top-width`等，规定具体方向上的边框宽度

border-color用于指定四个方向的颜色，同样支持拆分语法

border-style用于指定边框的样式，solid实线、dotted点状虚线、dashed段状虚线、double双线，同样支持拆分语法

:candy:border的简写语法允许组合`width color style`顺序可以互换，支持拆分语法

## 内边距 padding

内边距即内容区和边框之间的距离

一共有四个方向的内边距 padding-top padding-right padding-bottom padding-left

:warning:内边距的设置会影响到盒子的大小；背景颜色默认情况下会延伸到内边距上

一个盒子的**可见大小**，包括内容区、内边距、边框三部分

![image-20210510214908647](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210510214908647.png)

> 如图内容大小为`50*50`，而可见大小为`110*110`

### 外边距 margin

* 外边距不会影响盒子可见框的大小
* 但是外边距会影响盒子的位置
* 一共有四个方向的外边距：上 右 下 左
* 设置正值，元素会往反方向平移（因为增加了该方向的对外距离）
* 设置负值，元素会往目标方向移动（因为坍塌了该方向的对外距离）

> 元素在页面中是按照自左向右的、自上而下排列的，所以默认情况下，如果设置的是左和上外边距，则会将会移动元素自身，而设置下和右边距会移动其他元素

一个盒子的**占地大小**，包括内容区、内边距、边框、外边距

## 水平布局

元素在其父元素中水平方向的位置由以下几个属性共同决定

* margin-left
* border-left
* padding-left
* width
* padding-right
* border-right
* margin-right

**A:子元素以上所有属性值的总和应该等于父元素内容区的宽度**

当块级元素在父元素中未满足条件A且`margin-left``width``margin-right`属性中没有`auto`时，此元素会被**过渡约束**，自动补充margin-right以适应父元素的宽度（F12审查元素的橙色就是自动补充的）

（此时设置margin-right也无效）

如果`margin-left` `width` `margin-right`属性中有`auto`且为满足条件A时，则自动调整`auto`等于父元素的内容宽度

例子：父元素的内容宽为800px

设置子元素为

```css
.inner{
    width:auto;
    margin-left:100px;
    margin-right:200px;
}
```

100 + auto + 200 = 800 ，因此width最后被调整为为500px

![image-20210513214010597](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210513214010597.png)

> width的值默认就是auto，所以此处省略auto结果一样

> width的优先级高于margin，预留的空间会优先分配给width；三个属性都设置为auto，margin归零，width撑满

> 如果margin-left和margin-right设置为auto，而width固定，则子元素在其父元素中水平居中

## 垂直布局

默认情况下，父元素的内容被子元素撑开

## 溢出

子元素是在父元素的内容区中排列的，如果子元素的高度或者高度超过了父元素，则子元素会从父元素溢出

这时可以通过`overflow`属性来处理，注意scroll是应用在父级元素上的

可选值有：

* visible   溢出的内容正常显示
* hidden  溢出的内容会被裁减掉，不显示
* scroll 生成两个滚动条，通过滚动条来查看内容
* auto 根据需要生成滚动条，不会生成多余的滚动条

> 可以通过`overflow-x`和`overflow-y`控制单方向的溢出情况

## 重叠

垂直方向的相邻**外边距**(margin)会发生重叠

### 兄弟情况

* box1 ——margin-bottom:100px
* box2—— margin-top:100px

如果box1恰好在box2上方，则这两个margin会在视觉上合并为100px高度（而非200px）

:pencil:比较规则

如果两者数值不同且为两正或两负，则取绝对值较大值；两者一正一负，取两者的代数和

> 兄弟元素之间外边距的重叠，对于开发有利，因此不需要进行处理

### 父子情况

* 父子元素之间外边距，子元素的上左外边距会直接突出影响到父元素
* 父子外边距会影响到页面的布局，必须要进行处理



## 行内元素的盒模型

行内元素不支持设置宽度和高度，其大小由其内容直接决定

行内元素可以设置padding和margin 但是垂直方向的padding和margin不会影响页面的布局

> 换行会让行内元素间会产生一个空隙，在一行内书写即可

display属性：

* block 块级元素
* inline 行内元素
* inline-block 行内块元素
* table 将元素设置为表格
* none 元素不在页面中显示

> inline-block 既可以设置宽度高度又不会独占一行

`visibility`用来控制元素的显示状态，可选值有 `visible`  `hidden` 

> visibility在页面中不显示，但是仍然处于文档流中（占位）

## 浏览器的默认样式

通常情况，浏览器会为元素设置一些默认样式

默认样式的存在，会影响到页面的布局，通常情况下，在编写样式时必须去除掉页面的默认样式

在所有样式开始前使用`*{margin:0;padding:0}`去掉默认的填充的外边距

:raised_hand_with_fingers_splayed:快捷修改

对默认样式进行了统一

[necolas/normalize.css: A modern alternative to CSS resets (github.com)](https://github.com/necolas/normalize.css)

去除掉了所有默认样式

[CSS Tools: Reset CSS (meyerweb.com)](https://meyerweb.com/eric/tools/css/reset/)

## 盒子细节

### 盒子的大小

`box-sizing`用来指定盒子大小的计算方式

* `content-box` width/height = content
* `border-box` width/height = content + padding + border

### 轮廓、 阴影、圆角

`outline`用来设置元素的轮廓线，用法和border一致，但是不占用空间

`box-shadow`用来指定盒子的阴影，不占用空间

> x偏移量 | y偏移量 | 阴影模糊半径（程度） | 阴影扩散半径 | 阴影颜色 
> x偏移量 | y偏移量 | 阴影模糊半径（程度） | 阴影颜色 
> 插页 ( 阴影向内 ) | x偏移量 | y偏移量 | 阴影颜色

`border-radius`用来指定盒子的圆角，其值为相切边角圆的半径，可以拆分为上下左右

> 如果设置为两个值，则切出来的圆角为椭圆的弧
>
> 设置为50%；盒子变成圆形

## 浮动布局

### 基础

通过浮动，可以使一个元素向其父元素的左侧或者右侧靠拢

通过float属性来设置`float:left` `float:right`方向；`float:none`不浮动

浮动的特点：

* 水平布局的等式不需要强制成立

* 元素完全从文档流中脱离，不再占用文档流的空间
* 浮动元素默认不会从父元素中溢出
* 浮动元素不会盖住其他的浮动元素，而是进行类似行内的方式排列（不管是left还是right)
* 浮动元素不会盖住文字，而是形成所谓的**文字环绕图片效果**

脱离文档流元素的特点：

* 块元素
  * 不再独占一行
  * 在默认情况下，宽高被内容撑开

* 行内元素
  * 可以设置宽高，和默认的块元素一样

> 可以视为元素变成了`inline-block`

### 高度塌陷

当父元素的高度是由子元素决定时而将子元素设置为浮动后，因为子元素脱离了文档流而导致父元素高度塌陷。

解决方法：

* 父元素高度固定
* BFC（Block Formatting Context）块级格式化上下文

1. BFC解决

BFC是一个CSS中隐含的属性

开启BFC以后，该元素会变成一个独立的布局区域，换其他的块不同

元素开启BFC的特点

* 开启BFC的元素不会被浮动元素所覆盖
* 开启BFC的元素，子元素和父元素的外边距不会重叠
* 开启BFC的元素可以包含浮动的子元素

可以通过一些特殊方式来开启元素的BFC

* 设置元素的浮动（不推荐）
* 将元素设置为行内元素（不推荐）
* 设置`oveflow`属性为非`visible`
* 等等

[块格式化上下文 - Web 开发者指南 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)

2. clear解决

如果不想希望某个元素因为其他元素浮动的影响而改变位置，则可以使用`clear:left/ clear:right/ clear:both`来清除某侧浮动对当前元素的影响



设置清除浮动以后，浏览器会自动会元素添加一个上外边距，以使其位置不受其他元素的影响

3. 伪类解决 （完美）

CSS的问题由CSS自己解决，在伪类中添加`clear:both`

## 实用类.clearfix

以下方法可以解决子**元素外边距传递**和**高度塌陷**问题

```css
.outer{
    width:200px;
    height:200px;
 	background-color:#bfa;
}
.inner{
    width:100px;
    height:100px;
    background-color:orange;
}
.clearfix::before,.clearfix::after{
    content:'';
    display:table;
    clear:both;
}
```

## 定位

使用`position`属性来设置定位，可选值有：

* static 默认值（关闭定位）
* relative（相对定位）
* absolute（绝对定位）
* fixed（固定定位）
* sticky（粘滞定位）

找坐标原点，给元素设置`left:0` `right:0`

### 相对定位 relative

相对定位的基准点是元素在**文档流中正常的位置。**

特点

* 如果不设置偏移量，元素不会发生任何变化
* 通过偏移量（offset）来改变元素的位置：相关属性为`top` `bottom` `left` `right`
* 只有开启定位，才能设置偏移量
* 定位垂直方向的属性，我们通常只会使用其中之一
* 偏移量可以为负值
* :star:相对定位不会脱离文档流
* 相对定位不会改变元素的性质

### 绝对定位 absolute

和相对定位的区别：

* 开启绝对定位后，如果不设置偏移量，则元素的位置不会发生变化
* 开启绝对定位后，如果不设置偏移量，则元素会从文档中脱离

特点

* 绝对定位会影响元素的性质，行内元素会变成`inline-block`
* 绝对定位会使元素提升一个层级



:star:绝对定位是相对于其**包含块**进行定位的

包含块 

* 正常情况下，包含块就是离当前元素最近的**祖先块元素**
* 开启绝对定位后，包含块就是离当前元素最近的**开启了定位的祖先块元素**
* 如果所有的祖先元素都没有开启定位，则相对于根元素进行定位



![image-20210516150929365](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210516150929365.png)



### 固定定位 fixed

固定定位也是一种绝对定位，大部分特点都和绝对定位一样

不同的是：

:star:固定定位永远参照**浏览器视窗**进行定位

> 视窗的大小是固定的，而`<html></html>`的大小不是固定的



与绝对的定位的不同：

* 无已定位祖先元素设置realtive，网页滚动，元素也会被滚动
* 对元素设置fixed，网页滚动，元素仍然在固定好的位



### 粘滞定位 sticky

当元素的position属性设置为sticky时，则开启了元素的粘滞定位，

粘滞定位和相对定位的特点基本一致，但是粘滞定位的元素到达某个位置时将自动转化为**固定定位**

意义：

* 未达到临界点，视条件相对于原始位置偏移
* 因为滚动而即将不满足定位条件，视条件固定在距离视窗边缘的某处
* 临界点可以做到四个方向各自突破临界点而互不影响

例：当box设置为top:100px left:50px而其上方正好有一个高度为100px的盒子时，则box只相对于原始位置向右移动50px，因为其在top方向达到了临界点(注意)

### 定位对水平布局的影响

**绝对定位**

总宽度 = left + marginLeft +  borderLeft + paddingLeft + width +paddingRight + marginRight + right

两种情况：

* 如果九个值中没有auto 则自动调整right使等式满足
* 如果有auto 则自动调整auto的值使等式满足
* 因为left和right默认值是auto 所以如果不知道left和right 则等式不满足时 会自动调整这两个值

> 垂直方向的高度也必须满足 = top+marginTop+paddingTop + borderTop + paddingBottom + borderBottom+marginBottm

可以利用这个特性来使得元素在父元素中水平垂直居中

## 层级

对于开启了定位的元素，可以通过z-index属性来指定元素的层级，z-index需要一个整数作为参数，值越大元素的层级越高，

层级优先级：

* 值越大元素的层级越高，元素的层级越高越优先显示

* 如果元素的层级一样，则优先选择显示新出现的元素
* 后代元素的层级比祖先元素高



## 字体 font

字体的相关属性

* color 前景色，主要用于设置字体颜色
* font-weight  

  * normal 默认

  * bold 加粗

  * 其他 ：不推荐
* font-style 
  * normal 正常

  * italic 斜体
* font-size 字体大小
  * em   相当于当前元素的一个font-size
  * rem   相当于根元素的一个font-size
* vertical-align :boom:
[https://www.cnblogs.com/starof/p/4512284.html](https://www.cnblogs.com/starof/p/4512284.html)
* font-family 字体族（指定字体的**类型**）:star:
> serif：衬线；sans-serif：非衬线字体；monospace：等宽字体

> 每一种字体类型有很多种具体的类型，这里只指定大类



:bulb: 同时指定多个字体，使用`,`隔开，字体生效时优先使用第一个，无法使用则顺延



:warning:如果在简写属性中不写以上属性，则系统会自动应用默认的值

### 自定义字体的方法

1. 下载字体，然后放入指定路径（如`./font/youyuan.ttf`

2. 在需要使用的样式表中声明字体族

   ```css
   @font-face {
   			/**指定字体的名称**/
   			font-family: 'myfont';
   			/**指定字体的路径**/
   			src: url('./font/youyuan.TTF');
   }
   ```

   

3. `p{font-famly:myfont}`来使用

> 在项目中使用自定义字体`@font-family`时需要注意 加载速度 和 版权问题

> 可以使用类似的方法将icon当做字体来使用，方便修改颜色和大小，节省空间



### 行高 line-height

行高指的是文字占有的实际高度，在没设置元素的高度的情况下，给元素内的字体设置行高，那么行高的大小就是此元素的高度。

行高的值：

* 具体像素
* 整数 ：`font-size`的倍数；这时border会紧贴文字（ 默认行高是1.333）

字体框：

* 字体框就是字体存在的格子，设置行高是在设置字体框的高度（和字体本身无关）
* 文字不会超过框的大小，但也不一定紧贴框

行高的分配：

* 沿着字体的中轴线上下均分

<img src="https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210522112036508.png" alt="image-20210522112036508" style="zoom: 67%;" />



行高也可以用来设置具体的行间距，因此如果想设置两行的间隔为一行，那么`line-heihgt`需要设置为2

`行间距 = 行高 - 字体大小`

<img src="https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210522112814830.png" alt="image-20210522112814830" style="zoom:67%;" />



### 文本

* text-align 对齐方式
  * base-line 基准
  * bottom 底部对齐
* text-decoration 文本装饰
* white-space 网页如何处理空白
  * normal 正常
  * nowrap 不换行
  * pre  保留空白
* text-overflow 文本溢出
  * ellipsis 省略号
  * clip 截断



## 背景

* background-color 背景色
* background-image 背景图片

> background-color和background-image可以同时设置（图片含有透明图层则可以看到效果）

>  如果背景图片大于元素，那么将会有一部分图片无法正常显示

* background-repeat 设置背景的重复方式
  * repeat 背景延伸着x轴 y轴双方向平铺占满盒子 :gear:
  * repeat-x；repeat-y 在指定方向上平铺
* background-position 背景图的九宫格位置 / 偏移量 

> 使用`left` `right` `bottom` `top` `center`指定背景图在九宫格中的位置，如左上为`top left` 需要写两个值，默认第二个值为`center`

> `background-position:10px 20px` 相对于元素的左上角，水平方向偏移10px，垂直方向偏移20px

* background-clip  背景色出现的范围
  * border-box  》覆盖content+padding+border
  * padding-box 》覆盖content+padding
  * content-box  》覆盖content

* background-origin 背景图片原点的位置

  * padding-box 》 padding 外沿 左上角 :gear:
  * border-box 》border 外沿 左上角
  * content-box 》 content 外沿 左上角

  <img src="https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210522170039616.png" alt="image-20210522170039616" style="zoom: 67%;" /><img src="C:/Users/Flynn/AppData/Roaming/Typora/typora-user-images/image-20210522170113348.png" alt="image-20210522170113348" style="zoom:67%;" /><img src="C:/Users/Flynn/AppData/Roaming/Typora/typora-user-images/image-20210522170219625.png" alt="image-20210522170219625" style="zoom:67%;" />

  

* background-size  背景图大小，第一个值表示高度 第二个值表示宽度

> 如果只写一个，则第二个值是`auto`

可以设置`background-size:100% 100%`来让图片适应盒子大小，但这样背景图会变形

可以通过以下属性来调整显示：

1. cover 优先将将盒子铺满，不变形，可能显示不全
2. contain 优先将图片完整显示，可能出现空白

* background-attachment
  * scroll 背景图片跟随元素滚动
  * fixed 背景图片位置固定

  

`background`简写语法的要求

* `background-position`在前，`background-size`在后
* `background-origin`在前，`background-clip`在后

### 渐变

渐变可以实现从一个颜色到其他颜色过渡的效果，渐变相当于图片

如`   background-image: linear-gradient(#38f, #bfa);` <开始颜色，结尾颜色>

![image-20210523143304316](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210523143304316.png)

> 也可以通过`background`简写来设置渐变色

可以指定渐变的方向，如从左往右`background-image: linear-gradient(to right, #38f, #bfa);`>

![image-20210523143458547](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210523143458547.png)

> to right; to bottom; to top; to left; 45deg（指定度数）；1 turn = 360deg（一圈）;

渐变可以指定多个颜色，默认情况下平均分配，两边接近纯色，中间作渐变处理

在颜色之后指定像素，可以约定当前颜色渐变的起始位置

`			background:linear-gradient(#38f 100px, #bfa 200px);`渐变出现的范围(100到200px)

**径向渐变**

`   background-image: radial-gradient(#38f, #bfa);`

<img src="https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210523151052765.png" alt="image-20210523151052765" style="zoom: 80%;" />

径向渐变的形状是根据元素的形状来计算的（正方形盒子 生成 圆形；长方形盒子 生成 椭圆形）



可以指定渐变的大小和位置，如`			background-image: radial-gradient(200px 300px at 0 0, #38f, #bfa);`

<img src="https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210523151859949.png" alt="image-20210523151859949" style="zoom:80%;" />

> at XX XX 是九宫格的语法，可以指定出现的任意位置，如右上 top right；也可以以左上为原点，设置其在x y轴方向的偏移量

## 过渡

过渡可以为一个元素在不同状态之间切换的时候定义不同的过渡效果。比如在不同的伪元素之间切换，像是 [`:hover`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:hover)，[`:active`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:active) 或者通过 JavaScript 实现的状态变化。

如：`transition:height 1s;`指定当此元素的**高度**发生变化需要**用时1s**完成。

过渡的详细属性： 

* transition-property   

   指定要执行过渡的属性，如`height`高度;`marginLeft`左外边距

> 多个属性间使用逗号`,`隔开，如果所有属性都需要过渡，则使用`all`关键字

> 大部分属性都支持过渡效果，只要值是可以计算的就能过渡

* transition-duration  

   过渡效果的持续时间(单位`1s` = `1000ms`)

> 如果在property使用逗号隔开了两个属性，则在这里可以使用逗号指定不同的过渡时间

* transition-timing-function 

   过渡的时序函数，指定的是动画的过渡执行方式（匀速、变速等）

  * ease    默认，慢速开始，先加速后减速
  * linear   匀速运动
  * ease-in   先慢后快
  * ease-out   先快后慢
  * ease-in-out   开始慢，中间快，结尾快
  * cubic-bezier()   使用贝塞尔曲线函数来手动指定时序
  * step(n, start/end)   分n步执行过渡效果（不是连续的）；start/end在指定时间即将**开始/结束**后执行分步

* transition-delay 

  等待一段时间后再执行过渡，单位(s/ms)

  

注意过渡必须是从一个有效值到另一个有效值进行过渡

>  如`auto`他不是一个有效数值，而许多属性的默认值为`auto`

简写属性`transition:XXXXXXX`，可以将以上属性都写到一行，如果要写延迟，则两个时间中的第一个是持续时间，第二个是延迟

![3](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/3.gif)

## 动画

动画过渡类型，都是可以实现一些动态的效果，不同的是过渡需要在某个属性发生变化时才会触发，动画可以自动触发动态效果。



要设置动画，必须要先设置一个关键帧。关键帧设置了动画执行的每一个步骤

```css
@keyframes 动画名{
    from{
        /*动画开始态*/
    }
    to{
        /*动画的结束态*/
    }
}
```

动画的相关属性：

* animation-name 动画名

* animation-duration 动画持续时间

* animation-delay 动画的延迟

* animation-iteration-count 动画执行的次数

* animation-timing-function 动画的时序函数，和transition中的一致

* animation-play-state 

  * running  默认值 动画执行
  * paused  动画暂停

* animation-fill-mode 动画的填充模式

  * none      默认值，动画执行完毕，元素回到初始位置（非动画首帧）
  * forwards:star:    元素停止在动画结束的位置
  * backwards     动画延时等待时，元素就会处于开始位置
  * both    同时设置`forwards`和`backwards`

* animation-direction 动画执行的方向

  * normal 从from到to
  * reverse 从from到to
  * alternate 首次从from到to，重复执行时每次执行的方向都相反
  * alternate-reverse 首次从to到from，重复执行时每次执行的方向都相反          

  

### 百分比关键帧

用于拆分动画的执行时间，可以增加任意区段

```css
	@keyframes ball {
			0% {
				margin-top: 0;
			}
			40%,60% {
				margin-top: 400px;
			}
			30,80% {
				margin-top: 350px;
			}

			100% {
				margin-top: 400px;
			}
		}
```

## 变形

HTML中的所有元素都存在坐标轴（x, y, z），变形就是通过CSS来改变元素的形状、角度和位置

:tada: 变形不会对元素所处的文档流产生任何影响

使用`transform`属性，可选值包含以下分类

### 平移

* 平移 
  * translateX()     沿着X轴方向平移
  * translateY()     沿着Y轴方向平移
  * translateZ()     沿着Z轴方向平移

我们在平移元素时，其百分比是相对于自身进行计算的

`transform:translateX(50%)`移动相当于自身**宽度**的50%



:bulb:利用平移可以让不定大小的元素水平垂直居中

1.默认情况下left和top设置的百分比是基于父元素的宽高

2.translateX Y 是相当于自身的宽高，负值则可以往自身左上角的反方向移动

```css
.box3 {
			position: absolute; /*当没有已定位的祖先元素，则相对于根元素上下左右居中*/
			width: 100px;
			height: 200px;
			background-color: orchid;
			left: 50%;
			top: 50%;
			transform: translateX(-50%) translateY(-50%);
}
```



<img src="https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210523203108007.png" alt="image-20210523203108007" style="zoom: 67%;" />



Z轴的平移处于立体效果（近大远小），但浏览器不支持透视，如果要看到，则需要设置网页的视距。

:heart:现在设置视距为800px `html{perspective: 800px;}`

* 当设置`   transform: translateZ(100px);` 元素适当扩大
* 当设置`   transform: translateZ(-100px);` 元素适当缩小
* 当设置`   transform: translateZ(800px);` 元素占满整个屏幕



:warning:一个元素最多有一个`transform`属性生效，因此需要将所有变形效果写在同一行中

### 旋转

通过旋转可以使元素沿着x y 或者 z 旋转指定的元素；正值表示顺时针

:warning:如果不设置视距，则不会产生透视效果

单位：deg 角度（1turn = 360deg)

* transform:rotateX() 沿x轴3d旋转
* transform:rotateY() 沿y轴3d旋转
* transform:rotateZ() 沿着z轴3d旋转（等同于rotate())



请注意，当rotate结合translate使用时，如果rotate属性在前，则会改变坐标轴的方向，其后的平移会沿着新的坐标轴进行

如box1 先转后平移，box2先平移后转

```css
	html {
			perspective: 800px;
		}
		.box {
			width: 200px;
			height: 200px;
			background-color: #bfa;
			margin: 100px auto;
		}

		.box2 {
			width: 200px;
			height: 200px;
			background-color: pink;
			margin: 100px auto;

		}

		.box:hover {
			transition: 1s;
			transform: rotateY(45deg) translateZ(100px);
		}

		.box2:hover {
			transition: 1s;
			transform: translateZ(100px) rotateY(45deg);
		}
```

请注意box1中的z轴方向发生了变化，而box2不变

![4](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/4.gif)

  ` backface-visibility: hidden`用于在旋转即将显示到背面时隐藏元素

### 缩放

涉及缩放的包含以下函数

* scale() 双方向缩放
* scaleX() 仅缩放垂直方向
* scaleY() 仅缩放垂直方向
* scaleZ() 仅缩放突出方向

1表示相当于元素的1倍，支持小数

> scaleZ的效果在3d变换下可以看出效果

使用`transform-origin`来指定缩放的原点（默认在元素自身的中点，等同于`transform-origin:50% 50%;`）



## 弹性盒 flexbox

flex由叫弹性盒，伸缩盒，是CSS3中的另一种布局手段，主要用于代替**浮动**

flex可以让元素具有弹性，让元素的大小和位置可以跟随页面的大小改变而改变

将元素设置为**弹性容器**

*  `display:flex` 块级弹性容器 ✔️
* `display:inline-flex ` 行内弹性容器

`flex`会让容器独占一行，`inline-flex`相当于把此容器设置为`inline-block`

**弹性元素**：弹性容器的**直接子元素**是弹性元素，又称弹性项

> 弹性元素也可以是弹性容器，flex可以相互嵌套

### 排列方向

* flex-direction
  * row   默认值  水平（左向右）
  * row-reverse  水平反向（右向左）(row的镜像)
  * column  垂直 （自上而下）
  * column-reverse 垂直反向（自下而上)

容器变为弹性盒后，弹性元素默认横向排列，使用`flex-direction`来指定弹性元素的排列方式：



弹性容器的排列方向称为**主轴**，主轴就是元素的排列方向；与主轴垂直的称为**侧轴**

如默认情况下主轴为`+x`,则侧轴为`+y`



### 伸缩和默认值

将弹性元素根据父元素的剩余空间进行延展

* flex-grow 
  * 0 不伸展
  * 1  均分父元素的**剩余空间**

用于指定元素伸展的系数

flex-grow可以对每个弹性元素单独设置，如现在ul中有三个li，分别设置`flex-grow`为1,2,2,

如果将li的宽度去掉，则这三个li的大小比例为`1:2:2`

* flex-shrink 
  * 0 不收缩
  * 1 均分收缩
  * 比例

当父元素中的空间不足以容纳所有子元素时，按照何种方式进行收缩

> 缩放是根据缩放系数和弹性盒大小决定的

:pencil2:使用`flex-basis`指定是元素在主轴上的基础长度

* 如果主轴是横向的 则该值指定的是元素的长度
* 如果主轴是纵向的 则该值指定的是元素的高度
* 默认值是auto ，表示参考元素自身的大小
* 如果传递了一个具体的数值，则以该值为准

flex属性也可以简写为 `flex: 增长系数 缩减系数 基础大小`



:tada:设置`flex-basis`可以避免溢出；`width`比`flex-basis`优先级更高



1.例子：

flex-basis为50px,弹性盒宽度为400px；设置flex-grow比例为1 ： 2 ： 3 ，效果如下

![image-20210530093649637](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210530093649637.png)

正常剩余的空间为（400-3*50) = 250px

`1:2:3`分配为`42:84:125`

因此1的最终大小为42+50=92；2的最终大小为84+50=134；3的最终大小为50+125=175

2.例子2：

flex-basis为100px,弹性盒宽度为200px；设置flex-shrink比例为1 ： 2 ： 3 ，效果如下



![image-20210530095352612](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210530095352612.png)

正常剩余的空间为（200px - 3*100px) = -100px

`1:2:3`分配为`-17:-34:-68`

因此1的最终大小为100-17=83；2的最终大小为100-34=66；3的最终大小为100-68=32

:pencil2:对弹性元素使用`order`属性来决定在父盒子中的显示顺序



## 视口和像素

:warning:像素（px）是一个相对单位，实际的显示大小由像素比决定，而像素比由显示器的设置（pc）、移动端的设置及其两者的分辨率决定，真正的相对单位是`dp`（和屏幕尺寸英寸直接挂钩）



视口是用来显示网页的区域，视口的大小可以改变



默认情况下，移动端的网页都会将视口设置为980px，如果超过会进行缩放



将像素比设置为最佳像素比（如iphone6为1:2）的视窗大小我们称为完美视口

Iphone6分辨率为750*1334，则需要在`meta`中设置为`content=375px`



以后在写移动端的页面时，需要在网页头中加入以下代码：

```css
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

自动将`html`的宽度设置为完美视口，并且设置初始化缩放为1倍

### vw 单位

对于移动端开发时 我们不能使用px来进行布局

100vw  = 一个视口的宽度

1vw = 1% 视口的宽度

vw的单位永远相对于视口宽度进行计算

如果设计图的100vw = 750px，则1vw = 0.1333px

所以40px需要40*0.1333 =5.332vw

## 媒体查询

媒体查询通常用来实现响应式布局：

* 网页可以根据不同的设备或窗口大小呈现出不同的效果
* 使用响应式布局   可以使一个网页适用于所有设备
* 通过媒体查询 可以为不同的设备 或者设备的不同状态来分别设置样式

语法：`@media 查询规则{}`

* all 所有设备
* print 打印设备
* screen 带屏幕的设备
* sppech 屏幕阅读器

> 可以使用逗号连接多个媒体类型

>  可以在媒体类型前添加only，其作用主要是为了**兼容**老版本的浏览器



添加宽度和高度，当满足条件时使用括号内的规则，width和height指定的是视窗的宽高

> 一般只设置宽度

* `@media (width:500px)` 当窗口自恰好为500px时应用样式
* `@media (min-width:500px) `至少为500px时应用样式 （>=500px)
* `@media (max-width:500px)`至多为500px时应用样式  (<=500px)

> 注意`min-width`和`max-width`在分界点（称为断点）也生效

常见设备的断点：

* 超小屏幕（移动设备） <= 768 px
* 小屏幕 (768-992 px)
* 中型屏幕 (992 - 1200 px )
* 大屏幕 (>=1200 px)

限制中型屏幕时样式生效（两个条件）

`@media (min-width:992px) and (max-width:1200px){}`

完整的写法：

`@media only screen (min-width:992px) and (max-width:1200px){}`

