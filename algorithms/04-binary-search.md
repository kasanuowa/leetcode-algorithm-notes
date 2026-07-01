# 04. 二分查找 Binary Search

## 1. 核心思想

二分查找的核心是：

> 如果搜索空间具有单调性，就可以每次砍掉一半不可能有答案的区间。

常见搜索空间：

```txt
有序数组
第一个满足条件的位置
最后一个满足条件的位置
最小可行值
最大可行值
```

## 2. 适用题型

看到这些信号，优先想二分：

```txt
有序
升序 / 降序
查找 target
插入位置
第一个大于等于
最后一个小于等于
O(log n)
单调条件
```

典型 LeetCode：

```txt
35. Search Insert Position
34. Find First and Last Position of Element in Sorted Array
69. Sqrt(x)
278. First Bad Version
704. Binary Search
875. Koko Eating Bananas
153. Find Minimum in Rotated Sorted Array
```

## 3. 经典例题和逐步拆解

### 例题：LeetCode 35. Search Insert Position

题意：

```txt
给一个升序数组 nums 和 target。
如果 target 存在，返回下标。
如果不存在，返回应该插入的位置。
```

示例：

```txt
nums = [1, 3, 5, 6]
target = 2
输出：1
```

### 思路

这题本质是：

```txt
找第一个 >= target 的位置
```

### 闭区间模板代码

```js
var searchInsert = function(nums, target) {
    let left = 0
    let right = nums.length - 1

    while (left <= right) {
        const mid = left + Math.floor((right - left) / 2)

        if (nums[mid] === target) return mid

        if (nums[mid] < target) {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }

    return left
};
```

### 逐步拆解

```txt
nums = [1, 3, 5, 6]
target = 2
```

初始：

```txt
left = 0
right = 3
```

第 1 轮：

```txt
mid = 1
nums[mid] = 3
3 > 2，所以 right = mid - 1 = 0
```

第 2 轮：

```txt
left = 0
right = 0
mid = 0
nums[mid] = 1
1 < 2，所以 left = mid + 1 = 1
```

循环结束：

```txt
left = 1
right = 0
```

返回：

```txt
left = 1
```

### 复杂度

```txt
时间复杂度：O(log n)
空间复杂度：O(1)
```

## 4. 易错点

### 易错点 1：mid 必须每轮重新计算

错误写法：

```js
let mid = Math.floor((left + right) / 2)

while (left <= right) {
    ...
}
```

如果 `left` 和 `right` 变了，但 `mid` 不更新，会死循环。

正确写法：

```js
while (left <= right) {
    const mid = left + Math.floor((right - left) / 2)
}
```

### 易错点 2：找不到时返回 left，不是 mid

在搜索插入位置题中：

```txt
循环结束时 left 就是插入位置
```

### 易错点 3：区间定义不能混

闭区间：

```txt
[left, right]
while (left <= right)
left = mid + 1
right = mid - 1
```

左闭右开：

```txt
[left, right)
while (left < right)
right = mid
left = mid + 1
```

新手阶段建议先写熟闭区间。

## 5. 今日练习题

### LeetCode 704. Binary Search

在升序数组中查找 target，存在返回下标，否则返回 -1。

提示：

```txt
这是最基础的闭区间二分。
```

参考模板：

```js
var search = function(nums, target) {
    let left = 0
    let right = nums.length - 1

    while (left <= right) {
        const mid = left + Math.floor((right - left) / 2)

        if (nums[mid] === target) return mid

        if (nums[mid] < target) {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }

    return -1
};
```

## 6. 精华总结

二分核心三问：

```txt
1. 搜索空间有没有单调性？
2. mid 判断后能砍掉哪一半？
3. left / right 更新会不会死循环？
```

一句话：

```txt
有序或单调，优先想二分。
```
