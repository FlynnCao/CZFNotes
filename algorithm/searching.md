# 搜索

## 二分查找

如前所述，二分查找是一种在每次比较之后将查找空间一分为二的算法。每次需要查找集合中的索引或元素时，都应该考虑二分查找。如果集合是无序的，我们可以总是在应用二分查找之前先对其进行排序。





1. ***预处理*** —— 如果集合未排序，则进行排序。
2. ***二分查找\*** —— 使用循环或递归在每次比较后将查找空间划分为两半。
3. ***后处理\*** —— 在剩余空间中确定可行的候选者。





主要思路：

* 设置超界条件为left>=right
* 每次取边界的中点和目标值进行比较
* 注意在取新的边界时应该使中点+1或-1，防止重复检查
* 在边界中直接将arr[mid]和目标值进行比较，然后进行后处理
* `mid=left+(right-left)/2`可以防止溢出

模板1

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var search = function (nums, target) {
	let left = 0
	let right = nums.length - 1
	let mid = 0
	while (left <= right) {

		mid = Math.ceil((left + (right - left)) / 2)
		if (target > nums[mid]) {
			left = mid + 1
		} else if (target < nums[mid]) {
			right = mid - 1
		} else {
			return mid
		}
	}
	return -1
};

console.log(search([-1, 0, 3, 5, 9, 12], 9))
```





