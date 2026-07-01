# 02. 双指针 Two Pointers

## 1. 核心思想

双指针就是用两个位置变量一起移动，减少无效枚举。

常见类型：

```txt
左右指针：left 从左边走，right 从右边走
快慢指针：fast 扫描，slow 维护结果区
```

常用变量：

```js
let left = 0
let right = nums.length - 1
```

或者：

```js
let slow = 0
for (let fast = 0; fast < nums.length; fast++) {}
```

## 2. 适用题型

看到这些信号，优先想双指针：

```txt
有序数组
反转字符串
判断回文
原地修改数组
移动元素
删除重复元素
两个数之和
```

典型 LeetCode：

```txt
26. Remove Duplicates from Sorted Array
27. Remove Element
125. Valid Palindrome
167. Two Sum II - Input Array Is Sorted
283. Move Zeroes
344. Reverse String
```

## 3. 经典例题和逐步拆解

### 例题：LeetCode 283. Move Zeroes

题意：

```txt
把所有 0 移动到数组末尾，同时保持非零元素相对顺序。
必须原地修改。
```

示例：

```txt
nums = [0, 1, 0, 3, 12]
输出：[1, 3, 12, 0, 0]
```

### 思路

定义：

```txt
fast：扫描整个数组
slow：下一个非零元素应该写入的位置
```

### 模板代码

```js
var moveZeroes = function(nums) {
    let slow = 0

    for (let fast = 0; fast < nums.length; fast++) {
        if (nums[fast] !== 0) {
            nums[slow] = nums[fast]
            slow++
        }
    }

    while (slow < nums.length) {
        nums[slow] = 0
        slow++
    }
};
```

### 逐步拆解

```txt
nums = [0, 1, 0, 3, 12]
```

第一轮整理非零元素：

```txt
遇到 1：写到 nums[0]
遇到 3：写到 nums[1]
遇到 12：写到 nums[2]
```

此时前半部分是：

```txt
[1, 3, 12, _, _]
```

然后从 `slow` 开始补 0：

```txt
[1, 3, 12, 0, 0]
```

### 复杂度

```txt
时间复杂度：O(n)
空间复杂度：O(1)
```

## 4. 易错点

### 易错点 1：不理解 slow 的含义

`slow` 不是当前遍历位置。

它表示：

```txt
结果区下一个应该写入的位置
```

### 易错点 2：题目要求原地修改，不需要返回数组

LeetCode 283 的函数说明：

```txt
Do not return anything, modify nums in-place instead.
```

所以可以不写：

```js
return nums
```

### 易错点 3：无序数组不要乱用左右夹逼

无序 Two Sum 用哈希表。

有序 Two Sum 才适合左右双指针。

## 5. 今日练习题

### LeetCode 344. Reverse String

原地反转字符数组。

提示：

```txt
left 指向开头
right 指向结尾
left < right 时交换
```

参考模板：

```js
var reverseString = function(s) {
    let left = 0
    let right = s.length - 1

    while (left < right) {
        ;[s[left], s[right]] = [s[right], s[left]]
        left++
        right--
    }
};
```

## 6. 精华总结

双指针常见两类：

```txt
左右指针：从两边向中间收缩
快慢指针：fast 扫描，slow 维护结果
```

一句话：

```txt
有序数组左右夹，原地整理快慢指。
```
