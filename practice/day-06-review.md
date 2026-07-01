# Day 6 综合练习课

复习专题：

```txt
哈希表
双指针
滑动窗口
二分查找
栈
```

## 1. 核心思想

今天训练的不是新算法，而是：

```txt
看到题目，先判断题型，再套对应模板。
```

判断树：

```txt
要快速查之前有没有？ → 哈希表
数组有序 / 原地整理 / 左右收缩？ → 双指针
连续子数组 / 连续子串 / 最长最短？ → 滑动窗口
有序数组查位置 / 单调条件？ → 二分
最近未匹配 / 后进先出？ → 栈
```

## 2. 适用题型

| 专题 | 关键词 | 工具 |
|---|---|---|
| 哈希表 | 出现过、重复、频率、配对 | Map / Set |
| 双指针 | 有序、反转、原地修改、移动元素 | left / right / slow / fast |
| 滑动窗口 | 连续、最长、最短、窗口条件 | left / right / sum / window |
| 二分查找 | 有序、插入位置、O(log n) | left / right / mid |
| 栈 | 括号、抵消、最近未匹配 | stack |

## 3. 经典例题和逐步拆解

### 例题：LeetCode 209. Minimum Size Subarray Sum

关键词：

```txt
连续子数组
最短
sum >= target
```

判断：

```txt
滑动窗口
```

代码：

```js
var minSubArrayLen = function(target, nums) {
    let left = 0
    let sum = 0
    let ans = Infinity

    for (let right = 0; right < nums.length; right++) {
        sum += nums[right]

        while (sum >= target) {
            ans = Math.min(ans, right - left + 1)
            sum -= nums[left]
            left++
        }
    }

    return ans === Infinity ? 0 : ans
};
```

关键点：

```txt
不是窗口一长就缩。
只有 sum >= target 时才缩。
```

## 4. 易错点

```txt
1. 无序 Two Sum 用哈希表，有序 Two Sum 用双指针。
2. 滑动窗口要明确什么时候移动 left。
3. 二分的 mid 必须每轮重新算。
4. 二分找插入位置，找不到时返回 left。
5. 栈题最后必须检查 stack.length === 0。
```

## 5. 今日练习题

### 1. LeetCode 217. Contains Duplicate

```txt
专题：哈希表
难度：Easy
训练目标：Set 判断重复元素
```

提示：

```txt
seen.has(num) 说明重复。
```

### 2. LeetCode 1. Two Sum

```txt
专题：哈希表
难度：Easy
训练目标：Map 记录 值 -> 下标
```

提示：

```txt
先查 target - nums[i]，再存 nums[i]。
```

### 3. LeetCode 242. Valid Anagram

```txt
专题：哈希表 / 频率统计
难度：Easy
训练目标：字符计数和抵消
```

提示：

```txt
s 加次数，t 减次数。
```

### 4. LeetCode 344. Reverse String

```txt
专题：双指针
难度：Easy
训练目标：左右交换
```

提示：

```txt
left < right 时交换。
```

### 5. LeetCode 283. Move Zeroes

```txt
专题：双指针 / 快慢指针
难度：Easy
训练目标：slow 维护非零区
```

提示：

```txt
slow 是下一个非零元素写入位置。
```

### 6. LeetCode 209. Minimum Size Subarray Sum

```txt
专题：滑动窗口
难度：Medium
训练目标：最短满足窗口
```

提示：

```txt
sum >= target 时更新答案并缩小窗口。
```

### 7. LeetCode 35. Search Insert Position

```txt
专题：二分查找
难度：Easy
训练目标：找第一个 >= target 的位置
```

提示：

```txt
找不到时返回 left。
```

### 8. LeetCode 20. Valid Parentheses

```txt
专题：栈
难度：Easy
训练目标：最近未匹配元素
```

提示：

```txt
左括号入栈，右括号匹配栈顶。
```

## 6. 精华总结

推荐顺序：

```txt
217 → 1 → 242 → 344 → 283 → 35 → 20 → 209
```

复盘四问：

```txt
1. 我为什么判断它是这个专题？
2. 我的 left / right / slow / fast 分别代表什么？
3. 我什么时候更新答案？
4. 有没有处理边界？
```

一句话：

```txt
先判断题型，再套模板，最后处理边界。
```
