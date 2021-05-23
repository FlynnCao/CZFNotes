# 排序

## 冒泡排序

主要思路：以升序为例

* 每次将当前区域中最大的数赶到最右侧
* 下次缩小区域，右边界左移，然后将次大的数排到最右侧
* 这样只需要比较len-1次，因为最后只剩一个最小的数，无须比较（len为元素个数）

代码

```js
var search = function (nums) {
	let i = nums.length
	let j
	while (i > 0) {
		for (j = 0; j < i - 1; j++) {
			if (nums[j] > nums[j + 1]) {
				temp = nums[j]
				nums[j] = nums[j + 1]
				nums[j + 1] = temp
			}
		}
		i--
	}
	return nums
};
console.log(search([3, 1, 2, 4, 6, 10, 0]));
// ->  [0, 1, 2, 3, 4, 6, 10]
```

除了

