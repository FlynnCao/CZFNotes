# html、javascript、vue中的引号及变量嵌套

1.在常规需要使用变量的区域，无论外部的引号如何，使用`'"+变量名+"'`嵌套使用，相当于模板字符串的`${}`

```js
      var a = '大千世界';
      var str = "我喜欢'" + a + "'";
      var tem = "<p @click='fun1('" + a + "')'></p>";
      console.log(str); // => 我喜欢大千世界
	  console.log(tem); // => <p @click='fun1('大千世界')'></p>
```

> 在vue中，使用v-on模板绑定的事件`""`体内是javascript的写法

