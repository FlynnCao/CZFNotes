[toc]



# switch

使用checkbox实现，  原生的vuejs可以绑定表单元素传入布尔值 这里只需要注意特殊情况的处理 增加额外功能 修改样式即可



单个复选框，绑定到布尔值：





## created

假设此时未指定Switch组件选中与否的类型（即为boolean)，则当初值value传入true false 和 不合法类型时输出值如下：

```js
  console.log(!~[true, false].indexOf(true)); // -> false
    console.log(!~[true, false].indexOf(false)); // -> false 
    console.log(!~[true, false].indexOf('dwadaw')); // -> true
```

当输入不合格式的值时，关闭按钮

> :true-value 和 :false-value使用了vue的表单输入绑定来指定复选框的选中与否的判定形式，而非布尔型

## handleChange方法

如果不是选中状态 则修改为onValue->发出true 如果当前是选中状态 

请注意input和change事件的监控并不会返回 true或false 

![Snipaste_2021-04-29_17-23-17](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/Snipaste_2021-04-29_17-23-17.png)

 在不传入任何属性的情况下 this.value 返回 false 用户点击input后 通过input和change事件分发true(或者onValue指定的类型)

```js
this.$nextTick(() => {
      this.$refs.input.checked = this.checked;
   });
```

为了防止父元素拒绝修改组件的值 手动设置值同步 防止

这里使用nextTick的意义:等待真实dom形成后修改input的checked

> vue不会立即更新dom中的数据，而是开启一个新的队列，如果想要在一个方法内执行vm.a = 'xxx' 后获取a更新后的值，应当在 vm.nextTick回调中执行，此时新的dom已经渲染完毕

## 开关形态

[CSS](422a0ea6db905fb0d07dd0294b28d7bc.html) 属性 **`translate`** 允许你单独指定 transforms 中的平移，并独立于 [`transform`](dfde3315f169e295df1cd42e6ac8b543.html)法，并节省了在指定`transform` 值时必须记住的转换函数的确切顺序。

```js
    transform() {
        return this.checked ? `translate(${ this.coreWidth - 20 }px, 2px)` : 'translate(2px, 2px)';
      }
```

这里就完成了构造开关的过程 - 》圆点平移 -》 修改颜色

## 核心和文字标签

switch打开和关闭的文字随checked值更新，值得注意的是switch核心（开关本体）和文字的宽度保持一致

