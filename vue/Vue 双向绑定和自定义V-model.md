# Vue 双向绑定和自定义V-model

[toc]

## 双向绑定原理

父组件

```html
<template>
  <div class="about">
    <span>实现</span>
    <my-comp v-on:input="value = $event" v-bind:childValue="value"></my-comp>
    父组件{{value}}
    <!--父组件的value-->
    <input type="text" v-model="value">
  </div>
</template>
<script>
import myComp from './myComp/index'
export default {
  components: {
    myComp
  },
  data() {
    return {
      value: ' initial value'
    }
  }

}
</script>

```
子组件
```html
<template>
  <div>
    <input type="text" @input="$emit('input', $event.target.value)" v-bind:childValue="value">
  </div>
</template>
<script>
export default {
  props: ['childValue'],
  name: 'myComp',
}
</script>

```

>  v-on:input 联想为原生的 addEventListener方法

1.子组件通过`$emit`分发当前组件的输入值`$event.target.value`给父组件

2.父组件维护一个value状态

3.由于事件的冒泡机制，父组件也能接收到input事件的变动，`$event`这时会返回子组件传回的值，赋值给父组件的value状态，至此父组件可以监视子组件内输入值的变化

:warning:此时是单项绑定，如果再定义一个input将其model绑定在父组件的value里，修改这个Input内的值，子组件的input值不发生变化

1.使用props在子组件内注册一个自定义属性为`childValue`供父组件传入值

2.将自定义属性的值单向绑定到



![](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/20210415143123.png)

双向绑定后，两个input框内的值同步变化，效果一致

![](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/20210415143329.png)

## 自定义v-model

当我们需要为一个不是表单元素的组件设置v-model时，可以使用`model`语法

例如我们想封装element-ui已经提供好的搜索框作为新的搜索选项或者用于修改样式

结合v-model实现element中的checkbox



子组件 originImageCheckbox

```
<template>
  <div>
    <input type="checkbox" :value="data" @change="$emit('change', $event.target.checked)">
  </div>

</template>
<script>
export default {
  model: {
    prop: 'data', //绑定属性
    event: 'change' //抛出事件
  },
  props: { //定义属性
    data: {
      type: Boolean
    }
  },

}
</script>

<style lang="scss">
</style>
```

父组件使用：

```html
<template>
	<origin-image-checkbox v-model="myChecked"></origin-image-checkbox>
</template>
<script>
	export default{
        data(){
            return {
				myChecked:false	
            }
        }
    }
</script>
```

