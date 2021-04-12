# dev_ideas

### 生成扁平数组

```javascript
   getRouter(arr, des) {
      for (let index = 0; index < arr.length; index++) {
        arr[index].ChildNodes.length == 0 ? des.push({ id: arr[index].id, url: arr[index].urlAddress }) : this.getRouter(arr[index].ChildNodes, des)
      }
    },
```

