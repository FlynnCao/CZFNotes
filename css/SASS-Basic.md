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

## 数据类型

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

