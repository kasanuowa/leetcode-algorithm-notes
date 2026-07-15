# 14. 前缀和 Prefix Sum

## 1. 核心思想

前缀和提前保存从数组开头到当前位置的累计结果，让连续区间求和从 `O(n)` 降到 `O(1)`。

推荐让前缀和数组比原数组多一位：

```js
const prefix = new Array(nums.length + 1).fill(0)

for (let i = 0; i < nums.length; i++) {
    prefix[i + 1] = prefix[i] + nums[i]
}
```

区间 `[left, right]` 的和：

```js
prefix[right + 1] - prefix[left]
```

## 2. 适用题型

```txt
多次查询区间和
左右两侧元素和
连续子数组和等于 K
统计区间内某类元素数量
二维矩阵区域和
```

典型 LeetCode：

```txt
303. Range Sum Query - Immutable
304. Range Sum Query 2D - Immutable
560. Subarray Sum Equals K
724. Find Pivot Index
```

## 3. 经典例题和逐步拆解

### LeetCode 724. Find Pivot Index

中心下标要求：

```txt
左侧元素和 === 右侧元素和
```

先计算数组总和 `total`，遍历时动态维护当前位置左侧的 `leftSum`：

```js
var pivotIndex = function(nums) {
    let total = 0

    for (const num of nums) {
        total += num
    }

    let leftSum = 0

    for (let i = 0; i < nums.length; i++) {
        const rightSum = total - leftSum - nums[i]

        if (leftSum === rightSum) {
            return i
        }

        leftSum += nums[i]
    }

    return -1
}
```

逐步理解：

```txt
判断下标 i 时，leftSum 只能包含 i 左边的元素。
右侧和 = total - leftSum - nums[i]。
比较完成后，才把 nums[i] 加入 leftSum，供下一个位置使用。
```

复杂度：

```txt
时间复杂度：O(n)
空间复杂度：O(1)
```

## 4. 易错点

```txt
1. 判断中心位置前就把 nums[i] 加入 leftSum，把当前元素算进左边。
2. 区间公式漏掉 right + 1。
3. 前缀和处理的是连续区间，不是任意挑选的子序列。
4. 数组含负数时，普通滑动窗口可能失效，前缀和仍然可用。
5. 前缀和 + Map 题中，Map 往往需要记录出现次数，而不是只记录是否出现。
6. 和为 K 的子数组必须初始化 map.set(0, 1)，避免漏掉从下标 0 开始的答案。
```

## 5. 今日练习题

### LeetCode 560. Subarray Sum Equals K

```txt
难度：Medium
训练目标：前缀和 + 哈希表、统计多个起点
```

核心关系：

```txt
当前前缀和为 sum。
如果以前出现过 sum - k，那么两段前缀和之间的子数组和就是 k。
```

模板：

```js
var subarraySum = function(nums, k) {
    const map = new Map()
    map.set(0, 1)

    let sum = 0
    let count = 0

    for (const num of nums) {
        sum += num

        const need = sum - k
        count += map.get(need) || 0

        map.set(sum, (map.get(sum) || 0) + 1)
    }

    return count
}
```

注意：

```txt
count 加的是 need 此前出现的次数，不只是 count++。
同一个 need 出现多次，代表有多个不同的子数组起点。
```

## 6. 精华总结

```txt
连续区间和 = 两个前缀和之差。
多次区间查询使用 prefix 数组。
连续子数组和等于 K，常用前缀和 + Map。
关键初始化：prefix[0] = 0，或 map.set(0, 1)。
```
