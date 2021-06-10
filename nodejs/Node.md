# Node



## 概述

Nodejs可以解析JS代码（没有浏览器安全级别的限制）来提供很多系统级别的API，如：

* 网络通信

* 文件的读写

* 进程的管理

  

>  Nodejs不存在浏览器安全级别的限制，因此它可以轻松访问跨域的内容



## 模块

* 内置的模块
* 第三方的模块
* 自定义模块

commonjs是node第三方的规范，是针对后端的（服务器端）

> 浏览器是不支持`require()`语法的

### commonjs规范

定义模块->暴露模块接口->引用模块->使用模块

module.exports 语法

```js
/**sir.js**/
const my = {
    surname: '你的',
    sayName() {
        console.log(this.surname)
    }
}
const age = {
    age:100
}
module.exports.age = age
module.exports.my = my
/*app.js*/
const dodo = require('./sir.js') //必须写路径
dodo.my.sayName()
console.log(dodo.age.age)
```

`exports`相当于`module.exports`的引用，可以一次导出多个子模块

```js
/*sir.js*/
const my = {
    surname: '你的',
    sayName() {
        console.log(this.surname)
    }
}
const age = {
    age: 100
}
exports.default = {
    age, my
}
/*app.js*/
const dodo = require('./sir') 
dodo.default.my.sayName()
console.log(dodo.default.age.age)

```

一个模块可以被重复调用很多次

### nodejs的内置模块

[http://nodejs.cn/api/](http://nodejs.cn/api/)

**1.url**

**url.parse()**  :red_circle:

 解析传入的url，返回协议、域名、哈希值等信息（对象形式）

**url.format()** :red_circle:

将传入的url对象转化为相应的url字符串

**url.resolve()**  :red_circle:

转换成指定格式的url字符串

```js
const url = require('url');
url.resolve('/one/two/three', 'four');         // '/one/two/four'
url.resolve('http://example.com/', '/one');    // 'http://example.com/one'
url.resolve('http://example.com/one', '/two'); // 'http://example.com/two'
```

PS: `const urlParams = new URLSearchParams(url.parse(urlParams).search)`将网址最后跟的参数转化为对象，然后使用`get`方法获取对象值

:blue_heart:使用`log4js`插件可以将控制台的内容以日志形式存储在文件中，方便查看

`npm i log4js -D ` 安装依赖

```js
var log4js = require('log4js')
var url = require('url')
log4js.configure({
    appenders: {cheese: {type: "file", filename: "cheese.log"}},
    categories: {default: {appenders: ["cheese"], level: "error"}}
});
var logger = log4js.getLogger('cheese')
logger.level = 'debug'
logger.debug('XXXX') //会在当前目录下生成一个cheese.log来存放日志
```

2.querystring 查询字符串

**querystring.parse()**

将查询字符串转化为对象

**querystring.escape()**

将查询字符串进行百分比编码

**querstring.unescape()**

将百分比编码解码为查询字符串

**querystring.stringify()**

将对象序列化为查询字符串

> 当序列化对象中有不安全字符时（如中文）会自动转化为百分比编码，如果想要还原，可以为stringify指定选项

```js
let obj = { id: '2', name: 'tony', from: '北京' }
const newQuery = queryString.stringify(obj, null,null, {
    encodeURIComponent(str){
        return queryString.unescape(str)
    }			
})
console.log(newQuery) // -> id=2&name=tony&from=北京
//使用默认stringify方法 -> id=2&name=tony&from=%E5%8C%97%E4%BA%AC
```

### request&response前提

:crossed_flags:  get和post方法是前端发起请求，然后后端向前端返回数据。

>  get和post方法都能完成从浏览器到服务器的传参，get是以参数的形式，post是以body体的形式，

request = incoming message  浏览器发送过来的消息

response = ServerResponse  给浏览器返回的信息



node的浏览器端调试：

:tada:使用`node --inspect index.js`普通调试

:tada:使用`node --inspect --inspect-brk index.js`断点调试





**response**

`response.writeHead()`  控制给浏览器的响应头,包含以下参数

> 1.状态码（status-code） 200成功 404未找到

> 2.编码格式（content-type）默认格式为text/html，自动解析html标签

`reponse.write()` 向浏览器相应信息



:blue_heart:使用`supervisor` `nodemon`等进程管理工具来在后端进行实时刷新

### 创建服务器并监听

```js
const http = require('http')
const server = http.createServer(((req, res) => {})
server.listen(8989,()=>{
    console.log('listening on port 8989')
})    
```

对浏览器访问的url进行简单处理

```js
http.createServer(function (req, res) {
	let pathname = url.parse(req.url).pathname; //主机地址后的url
    pathname = pathname == '/' ? '/index.html' : pathname;
    let extname = path.extname(pathname); //文件后缀
}

```

### get

浏览器默认使用的方式，get方式也可以传参，只需要在网址后以`?`开头进行拼接即可，

在createServer方法中接收并处理浏览器传回的数据

```js
if(pathname == 'news'){
			var query = url.parse(req.url)
            console.log(query.query)
            res.writeHead(200, {
                'content-type': 'text/html;charset="utf-8"'
            })
            console.log(query.query.username + ';' + query.query.password)
            res.end('get成功')
}
```

### post 

使用`postman ` `insomnia`等通讯工具模拟前端向后端发送数据，或者使用form表单，指定`method`和`action`来提交表单并跳转

在createServer方法中接收并处理浏览器传回的数据

```js
		if(pathname == 'doregister'){
               let postdata = ''
                req.on('data', chunk => {
                    postdata+= chunk
                })
                req.on('end', ()=>{
                    console.log(postdata)
                    res.end(postdata)
            })
        }
```



## EJS

动态网站实现的两种方式：前后端分离和后端渲染，EJS是后端渲染的一种方式

> 前后端分离不利于SEO（搜索引擎）优化，一般以移动端为主

安装EJS `npm i ejs --save`

创建ejs模板，指定渲染方式

```js
<ul>
    <%=items%>
    <%for(var i=0;i<items.length;i++){%>
    <li><%=items[i].title%></li>
    <%}%>
</ul>
```

在app.js中渲染ejs模板并回传到浏览器中

```js
if (pathname == '/login') {
            let msg = '数据库中获得的消息'
            let list = [{ title: '新闻111'},{ title: '新闻222'},{ title: '新闻333'}]
            ejs.renderFile('./views/login.ejs', {msg: msg, items: list}, (err, data) => {
                res.writeHead(200, {
                    'content-type': 'text/html;charset="utf-8"'
                })
                res.end(data)
            })

        }
```

## 封装

### 简易封装

将`router.js`以commonjs规范的方式封装，然后在`app.js`中导入并调用

**router.js**

```js
let app = {
    static:(req,res,staticPath)=>{
        //静态页面逻辑
    },
    login:(req,res)=>{
        //登录逻辑
    },error(req,res)=>{
    	res.end('error')
	}
}
module.exports = app
```

**app.js** 引用路由并且创建服务器

```js
http.createServer(function (req, res) {
    routers.static(req,res,'static')
    let realpathname = url.parse(req.url).pathname.replace('/', '')
   try{
       routers[realpathname](req,res)
   }catch (e) {
        routers['error'](req,res)
   }
}).listen(3000);
```

### 对get和post分别进行处理

1.创建路由调度中心`app`

2.创建代理对象`G`

3.在调度中心下对`get`和`post`引入不同的方法

4.仅对于get方法使用调度中心处理注册

![image-20210606194529283](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210606194529283.png)

对于本次进程来说，首先 router的get方法被调用，传入`/login`和携带`req`和`res`对象的方法体

`router`以下对应`app`

app在`createServer`创建时被传入，因此可以使用request和response对象



router的get方法就是app的get方法

![image-20210606194954625](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210606194954625.png)

这时/login和req,res以及函数体被传入，输出`get方法`文本，然后使用代理对象G进行注册

代理对象G 查询 `/login`，并把键设置为`login`值设置为cb(函数体和其res req参数)

![image-20210606195418237](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210606195418237.png)

再次轮训到app主函数，将刚刚代理对象注册的方法执行，在浏览器打印出文字

![image-20210606195403196](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/image-20210606195403196.png)