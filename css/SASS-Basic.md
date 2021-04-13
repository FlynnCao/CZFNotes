# 介绍

可以将Sass理解为CSS的一个超集（类似于TypeScript)；属于<u>弱类型语言</u>

> 远古css无法嵌套书写导致代码冗杂繁重逻辑混乱；没有变量和样式复用机制

## CSS预处理器

1.SCSS/SASS

> scss是新版本的sass语法，兼容css，两者可以使用使用`@import`相互导入

2.LESS

> 相比编程性不够丰富

3.Stylus

> 大我

## SCSS的优点

- 完全兼容CSS3
- 在CSS基础上增加变量、嵌套、混合等功能
- 通过函数进行颜色值和属性值的运算
- 提供控制指令（control directives）等高级功能
- 自定义输出格式

# 环境

## 条条大道通罗马

练习场景推荐：a > b > c > d

**1，依赖编辑器**

a.  Node环境下的node-sass模块

b.  Node环境下的dart-sass模块 :rocket:

c.   Ruby环境下的sass模块

d .  Dart环境下的sass模块 :rocket:

**2，依赖编辑器**

vscode和webstorm的插件来编译sass

## 使用node-sass 

全局安装`npm install -g node-sass`

1. 单文件编译 `node-sass 源文件名.scss 目标文件名.scss`
2. 多文件（目录）编译 `-o`
3. 文件监听模式 `-w`


# ScssScript
## 注释

多行注释在编译后会附带到代码中，单行注释会被忽略

## 变量

### 定义和使用

变量以`$`开头，赋值方法与CSS属性的方法一致，正常标识符

```css
$my-width:100px;
$my-color:#323232;
#box{width:$my-width;background-color:}
```

### 作用域

变量支持块级作用域，嵌套规则内定义的变量只能在嵌套规则内使用（局部变量），不在嵌套规则内定义的变量则可在任何地方使用（全局变量）。将局部变量转换为全局变量可以添加 `!global` 声明

:warning:scss变量必须先定义后使用

> 推荐直接在scss的最顶部书写全局变量然后使用

[查看嵌套规则](#嵌套语法)

~~~scss
#foo {
  $width: 5em !global;
  width: $width;
}

#bar {
  width: $width;
}
~~~

## 数据类型 :cucumber:

* 数字 `1  2.5  13  10p!x 5a` 

* 字符串（有引号和无引号）  `'foo' "bar" baz`  :star:

  > 例如`color:red`和`font-family:'Helvetica'`

* 颜色 `blue #040302 rgba(255,110,223,0.5)`  :star:

  CSS原有颜色类型，十六进制、RGB、RGBA、HSL、HSLA和色彩单词

  SCSS提供了内置Colors函数，从而更方便地使用颜色

  ```scss
  $color0: green;
  $color1: lighten($color, 15%);
  $color2: darken($color, 15%);
  $color3: saturate($color, 15%);
  $color4: desaturate($color, 15%);
  $color5: (green + red);
  ```

* 布尔值 true false

  > 只有值为false和null才视为false，其他均视为true

* 空值 `null`

  > 变量的值为null时，编译时会被忽略

* 数组(list) `1.5em 1em 0.2em Helvetica, Arial, san-serif  :star:

  > 使用空格或者逗号分隔

   ``` scss
$list0: 1px 2px 5px 6px; //一个数组
$list1: 1px 2px, 5px 6px; //两个数组
$list2: (1px 2px)(5px 6px) //两个数组
   ```

* Maps （相当于JavaScript的Object) :star:

  ```css
  $total:(
      color:white,
      border:1px solid gray,
      background:black
  );
  ```
  

>  判断属性的方式 `type-of($value)`



## 运算

### 数字运算符

SassScript支持数字的加减乘除，取整等常规数学运算` + - * / %` 如有必要会在不同单位中进行转换

加法

* 数字相加，只要有单位结果必然有单位
* 字符串相加，第一个有引号结果就有引号，第一个无引号则结果无引号
* 数字和字符串相加，第一个有引号结尾必有引号

```
1 + 1 => 2
1 + 1px => 2px
1 + 'a' => 1a
"1" + a => "1a"
```

减法

* 每个字段的前半部位为数字则后半部分必须为单位

乘法

* 前半部分必须为数字，其中只有一个能含有单位

除法和取余

* 前半部分必须为数字且前者是纯数字时后者不能有字符
* 取余值和运算符之间一定要有空格，否则会被视为字符串

```scss
$num4:100/3+px;
$num5:10 % 3+px;
```

### 关系运算符

大前提：两端必须为数字

返回值：true或false

运算符：`>` `<` `>=` `<=` `==`

相等运算符： 会发生自动类型转换

```scss
$a:1 == 1px; //true
$b:"a" == a; //true
```

> ==  前半部分为非字符型数字进行比较时，单位会被自动忽略

### 布尔运算符

支持`and` `or`以及`not`运算

```scss
$a: 1 > 0 and 0 > 5; //false
```

> 值与布尔运算符之间必须有空格

### 颜色值运算 :heart:

- 颜色值是分段进行计算的，分别计算红色、绿色和蓝色的值，然后将结果进行拼接
- 颜色值的乘法也是分区域最后拼接得到结果
- 如果颜色值包含alpha channel，则需要保证透明度相等

```scss
$color:#010203 + #040506; // => #050709
$color:#010203 * 2; // => #020406
$color:rgba(0,1,0.5) + rgba(1,2,3,0.5); // => rgba(1,2,3,0.5)
```

### 运算符优先级

`()` 高于`*` `/` `%`高于  `+``-` 高于 `>` `<` `>=` `<=`

## 嵌套语法

这里说明嵌套语法

## 综合语法

### 插值

通过插值语法`#{}`可以在选择器、属性名和属性值中使用变量名

```scss
$name:foo;
$attr:border;
p.#{name}{
    #{attr}:1px solid red;
}
```

### 父选择器 &

通过`&`选择父级元素

```scss
a {
    color: yellow;
    &:hover{
        color: green;
    }
    &:active{
        color: blank;
    }
}
```

编译为

```css
a {
  color: yellow;
}

a:hover {
  color: green;
}

a:active {
  color: blank;
}
```

### !default

可以在变量的结尾添加 `!default` 给一个未通过 `!default` 声明赋值的变量赋值，此时，如果变量已经被赋值，不会再被重新赋值，但是如果变量还没有被赋值，则会被赋予新的值。

###  `!global`

可以将局部变量提升为全局变量

### `!optional` * 

如果 `@extend` 失败会收到错误提示，比如，这样写 `a.important {@extend .notice}`，当没有 `.notice` 选择器时，将会报错，只有 `h1.notice` 包含 `.notice` 时也会报错，因为 `h1` 与 `a` 冲突，会生成新的选择器。

如果要求 `@extend` 不生成新选择器，可以通过 `!optional` 声明达到这个目的.

简而言之：当`@extend`相关代码出现语法错误时，编译器可能会给我们"乱"编译为css，我们加上这个参数可以在出现问题后不让他编译该部分代码

## @-rules与指令

### @import

@import 被SASS拓展用于导入`scss`和`sass`文件，另外，被导入的文件中所包含的**变量**或者**混合指令 (mixin)** 都可以在导入的文件中使用。

如果想正确导入SCSS文件，应当使用文件名后缀或者忽略文件名

```scss
@import "foo.scss";
@import "foo";
// 以上两种方式均可

// 以下方式均不可行
@import "foo.css";
@import "foo" screen;
@import "http://foo.com/bar";
@import url(foo);
```

### @media

Sass 中 `@media` 指令与 CSS 中用法一样，只是增加了一点额外的功能：允许其在 CSS 规则中嵌套。如果 `@media` 嵌套在 CSS 规则内，编译时，`@media` 将被编译到文件的最外层，包含嵌套的父选择器。这个功能让 `@media` 用起来更方便，不需要重复使用选择器，也不会打乱 CSS 的书写流程。

@media`的 queries 允许互相嵌套使用，编译时，Sass 自动添加 `and`

`@media` 甚至可以使用 SassScript（比如变量，函数，以及运算符）代替条件的名称或者值

> 此处需要使用插值语法`#{}`



### @extend :star2:

@extend就是继承，主要用于scss代码的复用。

在设计网页的时候常常遇到这种情况：一个元素使用的样式与另一个元素完全相同，但又添加了额外的样式。

总的来看：支持层叠继承、多继承、允许延伸任何定义给单个元素的选择器（但是允许不一定好用）

```scss
.a{
    color:red;
}
.b{
   @extend .a;
   background-color: aquamarine;
}
.c{
    @extend .b;
}
```

最终结果

```css
.a, .b, .c {
  color: red;
}

.b, .c {
  background-color: aquamarine;
}
```

> 当合并选择器时，`@extend` 会很聪明地避免无谓的重复，，不能匹配任何元素的选择器也会删除。

#### 指令延伸限制

在指令中使用 `@extend` 时（比如在 `@media` 中）有一些限制：Sass 不可以将 `@media` 层外的 CSS 规则延伸给指令层内的 CSS

#### 选择器占位符

g.  `%placeholder`为选择器占位符，配合`@extend-Only选择器`使用。

效果：带有%符号的样式不会被编译，而是寻找使用@extend继承者复用其继承效果（包括前缀）

无前缀的情况

```css
// example1:
%img {
    color: red;
}
.path{
    @extend %img;
}
// 编译后：
.path {
  color: red;
}
```

有前缀的情况

```scss
// This ruleset won't be rendered on its own.
#context a%extreme {
  color: blue;
  font-weight: bold;
  font-size: 2em;
}
.notice {
  @extend %extreme;
}
//编译后
#context a.notice {
  color: blue;
  font-weight: bold;
  font-size: 2em; }
```

### @at-root

将代码置于最顶层

### @debug

便于调试

### @warn

用于警告

### @error

用于报错



## 控制指令

### if() 三元运算

表达式`if(expression, value1, value2)`

```scss
p{
    color:if(1+2 < 2, green, red)
}
```

编译为

```css
p {
  color: red;
}
```

### `@if` `@else` `@else if `条件语句

```scss
$a:1;
p{
    @if $a == 1 {
        color:blue
    }@else if $a == 2{
        color:red
    }@else{	
		color:green
    }
}
```

编译为

```css
p {
  color: blue;
}
```

> 当$a值为2时编译为 p{color:red}，其余为p{color:green}

### `@for`循环

循环语句

语法：`@for $var from <start> through <end>`或`@for $var from <start> to <end>`

> 使用through结束包含<end>的值，而使用to结束不包含<end>的值

```scss
@for $i from 1 through 3{
    .item-#{$i} {width:10px * $i} 
}
```

编译为

```css
.item-1 {
  width: 10px;
}

.item-2 {
  width: 20px;
}

.item-3 {
  width: 30px;
}
```

### `@while`循环

表达式 `@while expression`

`@while` 指令重复输出格式直到表达式返回结果为 `false`。这样可以实现比 `@for` 更复杂的循环，只是很少会用到

```scss
$i: 6;
@while $i > 0 {
    .item-#{$i} {
        width: 10px * $i;
    }
    $i: $i-2;
}
```

> 其中  $i: $i-2; 是赋值减语句

编译为

```css
.item-1 {
  width: 10px;
}

.item-2 {
  width: 20px;
}

.item-3 {
  width: 30px;
}
```

### @each 

*循环语句*

表达式：`$var in $vars`

`$var` 可以是任何变量名

`$vars` 只能是`Lists`或者`Maps`

* 一维列表

  ~~~scss
  @each $animal in puma, sea-slug, egret, salamander {
    .#{$animal}-icon {
      background-image: url('/images/#{$animal}.png');
    }
  }
  
  // compile:
  .puma-icon {
    background-image: url('/images/puma.png'); }
  .sea-slug-icon {
    background-image: url('/images/sea-slug.png'); }
  .egret-icon {
    background-image: url('/images/egret.png'); }
  .salamander-icon {
    background-image: url('/images/salamander.png'); }
  ~~~

- 二维列表 :warning:

  ~~~scss
  @each $first, $second, $third in (puma, black, default),
                                    (sea-slug, blue, pointer),
                                    (egret, white, move) {
    .#{$first}-icon {
      background-image: url('/images/#{$first}.png');
      border: 2px solid $second;
      cursor: $third;
    }
  }
  
  // compile:
  .puma-icon {
    background-image: url('/images/puma.png');
    border: 2px solid black;
    cursor: default; }
  .sea-slug-icon {
    background-image: url('/images/sea-slug.png');
    border: 2px solid blue;
    cursor: pointer; }
  .egret-icon {
    background-image: url('/images/egret.png');
    border: 2px solid white;
    cursor: move; }
  ~~~

  > 请注意这种方式每次将任意变量对应一个数组的任意个元素，然后下次循环读取下个数组的全部元素 

- maps对象

  ~~~scss
  @each $header, $size in (h1: 2em, h2: 1.5em, h3: 1.2em) {
    #{$header} {
      font-size: $size;
    }
  }
  
  // compile:
  h1 {
    font-size: 2em; }
  h2 {
    font-size: 1.5em; }
  h3 {
    font-size: 1.2em; }
  ~~~

## 混合指令

> 混合指令（Mixin）用于定义可重复使用的样式，避免了使用无语意的 class，比如 `.float-left`。混合指令可以包含所有的 CSS 规则，绝大部分 Sass 规则，甚至通过参数功能引入变量，输出多样化的样式。

注意：这不是函数！没有返回值！！

混合指令就是一大堆样式的集合，用来避免使用无意义的CSS

### 1.定义混合指令

混合指令的用法是在 `@mixin` 后添加名称与样式，以及需要的参数（可选）。

~~~scss
// 格式：
@mixin name {
    // 样式....
}
~~~

~~~scss
// example：
@mixin large-text {
  font: {
    family: Arial;
    size: 20px;
    weight: bold;
  }
  color: #ff0000;
}
~~~



### 2.引用混合样式

使用 `@include` 指令引用混合样式，格式是在其后添加混合名称，以及需要的参数（可选）。

~~~scss
// 格式：
@include name;

// 注：无参数或参数都有默认值时，带不带括号都可以
~~~

~~~scss
// example：
p {
    @include large-text;
}

// compile:
p {
  font-family: Arial;
  font-size: 20px;
  font-weight: bold;
  color: #ff0000;
}
~~~



### 3.参数

格式：按照变量的格式，通过逗号分隔，将参数写进Mixin名称后的圆括号里

支持默认值；支持多参数；支持不定参数；支持位置传参和关键词传参



> 括号以及内部的为形参，传入实参可以为其他变量名或者具体值



#### a. 位置传参

~~~scss
@mixin mp($width) {
    margin: $width;
}

body {
    @include mp(300px);
}
~~~



#### b.关键词传参

~~~scss
@mixin mp($width) {
    margin: $width;
}

body {
    @include mp($width: 300px);
}
~~~



#### c.参数默认值

~~~scss
@mixin mp($width: 500px) {
    margin: $width;
}

body {
    @include mp($width: 300px);
    // or
    @include mp(300px);
}
~~~



#### d.不定参数

> 官方：Variable Arguments
>
> 译文：参数变量
>
> 
>
> 有时，不能确定混合指令需要使用多少个参数。这时，可以使用参数变量 `…` 声明（写在参数的最后方）告诉 Sass 将这些参数视为值列表处理

~~~scss
@mixin mar($value...) {
    margin: $value;
}
~~~



### 4.先导入，再定义

在引用混合样式的时候，可以先将一段代码导入到混合指令中，然后再输出混合样式，额外导入的部分将出现在 `@content` 标志的地方

可以看作参数的升级版

~~~scss
@mixin large-text {
    .abc{
        @content
    }
}
@include large-text {
    display: flex;
    .logo {
        height: 100px;
    }
}


// compile:
.abc {
  display: flex;
}

.abc .logo {
  height: 100px;
}

~~~









------

## 十、函数指令

scss的下标从1开始

### 1.内置函数

#### a. 字符串函数

> 索引第一个为1，最后一个为-1；切片两边均为闭区间

| 函数名和参数类型                        |                  函数作用                   |
| :-------------------------------------- | :-----------------------------------------: |
| quote($string)                          |                  添加引号                   |
| unquote($string)                        |                  除去引号                   |
| to-lower-case($string)                  |                  变为小写                   |
| to-upper-case($string)                  |                  变为大写                   |
| str-length($string)                     |        返回$string的长度(汉字算一个)        |
| str-index($string，$substring)          |        返回$substring在$string的位置        |
| str-insert($string, $insert, $index)    |       在$string的$index处插入$insert        |
| str-slice($string, $start-at, $end-at） | 截取$string的$start-at和$end-at之间的字符串 |

> ster-insert的$index为插入的目标位置

例子：

```css
.b {
    font-family: to-upper-case(sass);
    font-family: to-lower-case("SASS");
    width: str-length("javascript");
    height: str-length("中国人民");
    font-size: str-index("abcabc", "ca");
    line-height: str-insert("iyou", "love", 2);
    line-height: str-slice("great", 1, 2);
}
//compile
.b {
  font-family: SASS;
  font-family: "sass";
  width: 10;
  height: 4;
  font-size: 3;
  line-height: "iloveyou";
  line-height: "gr";
}

```



#### b. 数字函数

| 函数名和参数类型        |                           函数作用                           |
| ----------------------- | :----------------------------------------------------------: |
| percentage($number)     |                       转换为百分比形式                       |
| round($number)          |                        四舍五入为整数                        |
| ceil($number)           |                         数值向上取整                         |
| floor($number)          |                         数值向下取整                         |
| abs($number)            |                          获取绝对值                          |
| min($number...)         |                          获取最小值                          |
| max($number...)         |                          获取最大值                          |
| random($number?:number) | 不传入值：获得0-1的随机数；传入正整数n：获得0-n的随机整数（左开右闭） |

例子

```css
.c{
    width: percentage(0.75);
    width: round(3.3434343);
    width: ceil(3.772);
    width: floor(3.772);
    width: abs(-1.332);
    width: min(7,3,1,5);
    width: max(0,6,10,2);
    height:random();
    height:random(10); 
}
//compile
.c {
  width: 75%;
  width: 3;
  width: 4;
  width: 3;
  width: 1.332;
  width: 1;
  width: 10;
  height: 0.19857;
  height: 9;
}
```



#### c. 数组函数

| 函数名和参数类型                 |                           函数作用                           |
| -------------------------------- | :----------------------------------------------------------: |
| length($list)                    |                         获取数组长度                         |
| nth($list, n)                    |                      获取指定下标的元素                      |
| set-nth($list, $n, $value)       |               向$list的$n处插入$value，替换之                |
| join($list1, $list2, $separator) | 拼接$list1和list2；$separator为新list的分隔符，默认为auto，可选择comma、space |
| append($list, $val, $separator)  | 向$list的末尾添加$val；$separator为新list的分隔符，默认为auto，可选择comma、space |
| index($list, $value)             |                返回$value值在$list中的索引值                 |
| zip($lists…)                     | 将几个列表结合成一个多维的列表；要求每个的列表个数值必须是相同的 |

```css
$my:zip(3 6 2 , 7 1 10, 2 2 3);
.d {
    width: length(3 1 2 5);
    width: nth(2 6 5 1, $n: 2);
    width: set-nth(1 3 2 5, 2, -1);
    width: join(1 3 5, 2 4 6, space);
    width: append(1 3 2, 5, space);
    height: zip(3 6 2 , 7 1 10, 2 2 3);
    line-height:length($my)
}
//compile
.d {
  width: 4;
  width: 6;
  width: 1 -1 2 5;
  width: 1 3 5 2 4 6;
  width: 1 3 2 5;
  height: 3 7 2, 6 1 2, 2 10 3;
  line-height: 3;
}
```

> $my为3*3的二维Number型数组

#### d. 映射函数

| 函数名和参数类型        |                 函数作用                 |
| ----------------------- | :--------------------------------------: |
| map-get($map, $key)     |        获取$map中$key对应的$value        |
| map-merge($map1, $map2) |     合并$map1和$map2，返回一个新$map     |
| map-remove($map, $key)  |     从$map中删除$key，返回一个新$map     |
| map-keys($map)          |            返回$map所有的$key            |
| map-values($map)        |           返回$map所有的$value           |
| map-has-key($map, $key) | 判断$map中是否存在$key，返回对应的布尔值 |
| keywords($args)         |  返回一个函数的参数，并可以动态修改其值  |



#### e. 颜色函数

- **RGB函数**

  | 函数名和参数类型               |                           函数作用                           |
  | ------------------------------ | :----------------------------------------------------------: |
  | rgb($red, $green, $blue)       |                     返回一个16进制颜色值                     |
  | rgba($red,$green,$blue,$alpha) | 返回一个rgba；$red,$green和$blue可被当作一个整体以颜色单词、hsl、rgb或16进制形式传入 |
  | red($color)                    |                   从$color中获取其中红色值                   |
  | green($color)                  |                   从$color中获取其中绿色值                   |
  | blue($color)                   |                   从$color中获取其中蓝色值                   |
  | mix($color1,$color2,$weight?)  |     按照$weight比例，将$color1和$color2混合为一个新颜色      |

- **HSL函数**

  | 函数名和参数类型                         | 函数作用                                                     |
  | ---------------------------------------- | ------------------------------------------------------------ |
  | hsl($hue,$saturation,$lightness)         | 通过色相（hue）、饱和度(saturation)和亮度（lightness）的值创建一个颜色 |
  | hsla($hue,$saturation,$lightness,$alpha) | 通过色相（hue）、饱和度(saturation)、亮度（lightness）和透明（alpha）的值创建一个颜色 |
  | saturation($color)                       | 从一个颜色中获取饱和度（saturation）值                       |
  | lightness($color)                        | 从一个颜色中获取亮度（lightness）值                          |
  | adjust-hue($color,$degrees)              | 通过改变一个颜色的色相值，创建一个新的颜色                   |
  | lighten($color,$amount)                  | 通过改变颜色的亮度值，让颜色变亮，创建一个新的颜色           |
  | darken($color,$amount)                   | 通过改变颜色的亮度值，让颜色变暗，创建一个新的颜色           |
  | hue($color)                              | 从一个颜色中获取亮度色相（hue）值                            |

- **Opacity函数**

  |                                                             |                  |
  | ----------------------------------------------------------- | ---------------- |
  | alpha($color)/opacity($color)                               | 获取颜色透明度值 |
  | rgba($color,$alpha)                                         | 改变颜色的透明度 |
  | opacify($color, $amount) / fade-in($color, $amount)         | 使颜色更不透明   |
  | transparentize($color, $amount) / fade-out($color, $amount) | 使颜色更加透明   |



#### f. Introspection函数

| 函数名和参数类型               |                           函数作用                           |
| ------------------------------ | :----------------------------------------------------------: |
| type-of($value)                |                       返回$value的类型                       |
| unit($number)                  |                      返回$number的单位                       |
| unitless($number)              |           判断$number是否带单位，返回对应的布尔值            |
| comparable($number1, $number2) | 判断$number1和$number2是否可以做加、减和合并，返回对应的布尔值 |





### 2.自定义函数

> Sass 支持自定义函数，并能在任何属性值或 Sass script 中使用
>
> Params: 与Mixin一致
>
> 
>
> 支持返回值

**基本格式：**

~~~scss
@function fn-name($params...) {
    @return $params;
}
~~~



~~~scss
// example:
@function fn-name($params...) {
    @return nth($params, 1);
}
p {
    height: fn-name(1px);
}

// compiled:
p {
  height: 1px;
}
~~~







------

## 十一、细节与展望

### 1.细节

a. @extend、@Mixin和@function的选择

[原文链接](https://csswizardry.com/2016/02/mixins-better-for-performance/)

![image-20200707171035353](https://raw.githubusercontent.com/ggdream/scss/master/sources.assets/image-20200707171035353.png)

> `minxins`在网络传输中比`@extend` 拥有更好的性能.尽管有些文件未压缩时更大，但使用`gzip`压缩后，依然可以保证我们拥有更好的性能。





**所以@extend我们就尽量不要使用了，而@Mixin和@function的差别在定义和使用上**



> 定义方式不同： `@function` 需要调用`@return`输出结果。而 @mixin则不需要。
>
> 使用方式不同：`@mixin` 使用`@include`引用，而 `@function` 使用小括号执行函数。







### 2.展望

>
>
>以上内容算是"基础"部分，但是对于日常开发，我觉得是足够使用的了。
>
>如果想要进一步了解，就必须先去学习下Ruby，使用Ruby相关模块进行更丰富地学习

### Unfinished...

