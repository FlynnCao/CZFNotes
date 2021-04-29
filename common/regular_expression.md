# ES6_3
[toc]
## 正则表达式
>  参考 https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions
### 概述
1、什么是正则表达式
正则表达式（regular expression）适用于匹配字符串中字符组合的模式。在JavaScript中，正则表达式也是对象。
2、正则表达式的作用
* **匹配**想要的字符串和文本
* 过滤页面中的一些敏感关键词 -- **替换**
* 从字符串中获想要的特定部分 -- **获取**

> 在实际开发中，我们一般都是复制直接写好的正则表达式，但是要求会根据实际情况来进行修改
### 在JavaScript中的使用
#### 创建正则表达式
1、通过调用RegExp的对象构造函数创建
```js
var regExp  = new RegExp(/123/);
console.log(regExp); // -> /123/
```
2、通过字面量创建
```js
var rg = /456/;  //-> 正则表达式字面量
console.log(rg); // -> 456
```
可以用`test()`方法来检测字符串是否符合该正则规则，返回布尔值
```js
var rg = /456/;
console.log(rg);
console.log(rg.test(456)); // ->true
console.log(rg.test('456')); // ->true
console.log(rg.test('abc')); // -> false
```
### 特殊字符
正则表达式可以是简单的字符和特殊字符的组合如`/ab*c/`，其中的特殊字符被称为元字符（在正则表达式中有特殊意义的符号）



#### 1、边界符
提示字符所处的位置，如`^`开始和`$`结尾
例子：修改先前的例子，渐进严格地使用不同的正则约束字符串
```js
        var rg = /abc/; //正则表达式里面不需要加引号，不管是数字型还是字符串型
        console.log(rg.test('abc')); // true
        console.log(rg.test('abcd')); // true
        console.log(rg.test('loveabc'));  //true
        console.log('-------------------');
        var rg2 = /^abc/;
        console.log(rg2.test('abc')); //true
        console.log(rg2.test('abcd')); //true
        console.log(rg2.test('xabc')); //false
        console.log('-------------------');
        var rg3 = /^abc$/;
        console.log(rg3.test('abc')); // true
        console.log(rg3.test('abcd')); //false
        console.log(rg3.test('xabc')); //false
```
#### 2、字符类
表示一系列的字符可供选择，只要匹配其中一个就行了
```js
         var rg = /[abc]/; //只要包含a或b或c都返回true
        console.log((rg.test('andy'))); //true
        console.log((rg.test('betty'))); //true
        console.log((rg.test('cathy'))); //true
        console.log((rg.test('red'))); //false
```
配合上文的边界符使用
```js
  var rg = /^[abc]$/; //只有是a或b或者c(作为开头及结尾)才返回true
        console.log(rg.test('abc')); //false
        console.log(rg.test('aa')); //false
        console.log(rg.test('a')); //true
        console.log(rg.test('b')); //true
        console.log(rg.test('c')); //true
```
如果我们想要限制一个范围内的单个字符都能通过，可以使用`-`来限定范围
```js
         var rg = /^[a-f]$/; //a~f这些单个字母都可以返回true
        console.log(rg.test('a')); // true
        console.log(rg.test('aa')); // false
        console.log(rg.test('1'));// false
        console.log(rg.test('A'));// false
        console.log(rg.test('g'));// false
```
如果我们想要让大写的A-F也能通过，可以使用如下方法来组合
```js
       var rg = /^[a-fA-F]$/; //a~f或者A-F这些字母都可以返回true
        console.log(rg.test('a')); // true
        console.log(rg.test('aa')); // false
        console.log(rg.test('1'));// false
        console.log(rg.test('A'));// true
        console.log(rg.test('G'));// false
```
可以用类似的方法进行组合验证，如用户名单个字符的验证可以写作`/^[a-zA-Z_-0-9]$/`

当 `^`作为第一个字符出现在一个字符集合模式时，表示取反（即目标字符串不能包含这些范围内的字符）
```js
         var rg = /^[^a-f]$/; 
        console.log(rg.test('a')); // false
        console.log(rg.test('aa')); // false
        console.log(rg.test('1'));// true
        console.log(rg.test('A'));// true
        console.log(rg.test('g'));// true
```
#### 2、量词符
量词符用来设定某个模式出现的次数，以下为常见的量词符：

`*`相当于>=0，可以重复0次或者多次
```js
        var reg = /^a*$/;
        console.log(reg.test('')); //true
        console.log(reg.test('a')); //true
        console.log(reg.test('aa')); //true
        console.log(reg.test('b')); //false
```
`+`相当于>=1次，至少要出现一次

```js
        var reg = /^a+$/;
       console.log(reg.test('')); //true
        console.log(reg.test('a')); //true
        console.log(reg.test('aa')); //true
        console.log(reg.test('b')); //false
```
`?`相当于1||0，允许出现1次或者0次

```js
        var reg = /^a?$/;
        console.log(reg.test('')); //true
        console.log(reg.test('a')); //true
        console.log(reg.test('aa')); //false
        console.log(reg.test('b')); //false
```
`{1,2}`就是只能重复1~2次
```js
        var reg = /^a{3}$/;
        console.log(reg.test('')); //false
        console.log(reg.test('a')); //true
        console.log(reg.test('aa')); //true
        console.log(reg.test('aaa')); //false
        console.log(reg.test('b')); //false
```
**综合应用：要求输入的用户名为字母、数字、下划线和分隔符，要求输入的字符长度范围在6-16之间**
```js
         var reg = /^[a-zA-z0-9_-]{6,16}$/;
        console.log(reg.test('19960713_caf')); //true
        console.log(reg.test('19960713_caf19960713')); //false
        console.log(reg.test('%&**@caf')); //false
```
#### 3、括号
* 中括号：字符集合，匹配方括号中的任意字符
* 大括号： 量词符，里面表示重复次数
* 小括号：表示提升优先级优先级
例子：我们想要只让`abcabcabc`通过
```js
         var reg4 = /^(abc){3}$/;
        console.log(reg4.test('abcabcabc')); //true
        console.log(reg4.test('abccc')); //
        console.log(reg4.test('abccc'));
        console.log(reg4.test('ab'));
```
### 预定义类（简写）
预定义类指的是**某些常见模式的简写方式**。

正则表达式中`|`表示或者
### 替换
`str,replace()`方法来实现字符串的替换，
第一个参数：被替换的字符串或者**正则表达式**
第二个参数：替换的字符串
返回值是一个替换完毕的新字符串

例子：将字符串中的敏感词自动替换为**
```js
var str = '老师讲课很有激情，希望能继续保持激情！'
var newStr = str.replace(/激情|gay|les/, '**');
console.log(newStr); // -> 老师讲课很有**，希望能继续保持激情！
```
正则表达式可以使用参数来进行更高级的配置，紧跟在正则表达式体之后
`/表达式/[switch]` 
* g 全局匹配
* i 忽略大小写
* gi 全局匹配+忽略大小写

例子：修改上文的敏感词替换算法，让后续的敏感词也能够成功被替换
```js
var str = '老师讲课很有激情，希望能继续保持激情！'
var newStr = str.replace(/激情|gay|les/g, '**');
console.log(newStr); // -> 老师讲课很有**，希望能继续保持**!
```
