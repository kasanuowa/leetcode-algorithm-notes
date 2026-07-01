# 01. 哈希表 Hash Table

## 1. 核心思想

哈希表的核心是：

> 用空间换时间，快速判断某个值、字符、状态之前是否出现过。

在 JavaScript 中常用：

```js
const set = new Set()
const map = new Map()
```

常见用途：

```txt
Set：判断是否出现过
Map：记录值对应的信息，比如下标、次数、位置
```

哈希表平均查找、插入复杂度通常是：

```txt
O(1)
```

所以它经常把暴力双循环 `O(n²)` 优化成 `O(n)`。

## 2. 适用题型

看到这些信号，优先想哈希表：

```txt
出现过
重复元素
两数配对
字符频率
异位词
统计次数
前缀状态
```

典型 LeetCode：

```txt
1. Two Sum
217. Contains Duplicate
242. Valid Anagram
349. Intersection of Two Arrays
383. Ransom Note
560. Subarray Sum Equals K
```

## 3. 经典例题和逐步拆解

### 例题：LeetCode 1. Two Sum

题意：

```txt
nums = [2, 7, 11, 15]
target = 9
```

返回两个下标，使得两个数之和等于 `target`。

### 思路

遍历到当前数 `x` 时，需要找：

```txt
target - x
```

这个数之前是否出现过。

### 模板代码

```js
var twoSum = function(nums, target) {
    const map = new Map()

    for (let i = 0; i < nums.length; i++) {
        const need = target - nums[i]

        if (map.has(need)) {
            return [map.get(need), i]
        }

        map.set(nums[i], i)
    }

    return []
};
```

### 逐步拆解

```txt
nums = [2, 7, 11, 15]
target = 9
```

第 1 轮：

```txt
当前数 2，需要 7
map 里没有 7
存入 2 -> 0
```

第 2 轮：

```txt
当前数 7，需要 2
map 里有 2，下标是 0
返回 [0, 1]
```

### 复杂度

```txt
时间复杂度：O(n)
空间复杂度：O(n)
```

## 4. 易错点

### 易错点 1：Two Sum 要先查再存

错误风险：

```js
map.set(nums[i], i)
if (map.has(target - nums[i])) ...
```

如果 `target = 6`，当前数是 `3`，可能自己和自己配对。

正确顺序：

```txt
先查 need
再存当前值
```

### 易错点 2：计数时初始值要处理好

推荐写法：

```js
map.set(char, (map.get(char) || 0) + 1)
```

### 易错点 3：`Map.size` 不是“次数是否为 0”

如果 key 还在 Map 里，即使 value 是 `0`，`size` 也不会变小。

如果想用 `map.size === 0` 判断，需要在次数归零时：

```js
map.delete(char)
```

## 5. 今日练习题

### LeetCode 242. Valid Anagram

判断两个字符串是否是字母异位词。

提示：

```txt
1. 长度不同，直接 false
2. 统计 s 中每个字符出现次数
3. 用 t 中字符逐个抵消
4. 最后所有计数都应该为 0，或者 Map 被清空
```

参考模板：

```js
var isAnagram = function(s, t) {
    if (s.length !== t.length) return false

    const map = new Map()

    for (const char of s) {
        map.set(char, (map.get(char) || 0) + 1)
    }

    for (const char of t) {
        if (!map.has(char)) return false

        const count = map.get(char) - 1

        if (count === 0) {
            map.delete(char)
        } else {
            map.set(char, count)
        }
    }

    return map.size === 0
};
```

## 6. 精华总结

哈希表题的第一反应：

```txt
我现在需要的东西，之前有没有出现过？
```

常见模板：

```txt
Set：查重复
Map：查配对 / 统计频率 / 记录下标
```

一句话：

```txt
要快速查历史状态，用哈希表。
```
