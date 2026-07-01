# 03. 滑动窗口 Sliding Window

## 1. 核心思想

滑动窗口维护一个连续区间：

```txt
[left, right]
```

其中：

```txt
right：扩大窗口
left：缩小窗口
```

本质是双指针的进阶版，专门处理连续子数组 / 连续子字符串。

## 2. 适用题型

看到这些信号，优先想滑动窗口：

```txt
连续子数组
连续子字符串
最长
最短
最多 K 个
不重复
窗口内频率
窗口内和
```

典型 LeetCode：

```txt
3. Longest Substring Without Repeating Characters
209. Minimum Size Subarray Sum
424. Longest Repeating Character Replacement
438. Find All Anagrams in a String
567. Permutation in String
1004. Max Consecutive Ones III
1456. Maximum Number of Vowels in a Substring of Given Length
```

## 3. 经典例题和逐步拆解

### 例题：LeetCode 209. Minimum Size Subarray Sum

题意：

```txt
target = 7
nums = [2, 3, 1, 2, 4, 3]
```

找和大于等于 `target` 的最短连续子数组长度。

### 思路

这是典型：

```txt
最短满足窗口
```

当窗口和 `sum >= target` 时，当前窗口满足条件。

这时应该：

```txt
1. 更新最短长度
2. 移动 left，尝试缩小窗口
```

### 模板代码

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

### 逐步拆解

输入：

```txt
target = 7
nums = [2, 3, 1, 2, 4, 3]
```

窗口走到：

```txt
[2, 3, 1, 2]
sum = 8
```

满足 `sum >= 7`，更新：

```txt
ans = 4
```

尝试缩小，移出左边的 `2`：

```txt
[3, 1, 2]
sum = 6
```

不满足，停止缩小。

后面窗口走到：

```txt
[4, 3]
sum = 7
```

更新：

```txt
ans = 2
```

最终答案：

```txt
2
```

### 复杂度

```txt
时间复杂度：O(n)
空间复杂度：O(1)
```

虽然有 while，但 left 和 right 都只往右走。

## 4. 易错点

### 易错点 1：窗口一长就缩，是错的

错误条件：

```js
while (left < right)
```

正确条件要看题目：

```js
while (sum >= target)
```

只有窗口满足条件，才缩小。

### 易错点 2：题目是 `sum >= target`，不是 `sum === target`

比如：

```txt
target = 7
nums = [8]
```

答案应该是 `1`。

### 易错点 3：最短窗口要先更新答案，再缩小

顺序：

```txt
while 满足条件：
  先更新答案
  再移出左边元素
```

## 5. 今日练习题

### LeetCode 3. Longest Substring Without Repeating Characters

找无重复字符的最长子串长度。

提示：

```txt
right 扩大窗口
用 Map 统计窗口内字符次数
如果当前字符重复，就移动 left 缩小窗口
窗口合法后更新最大长度
```

参考模板：

```js
var lengthOfLongestSubstring = function(s) {
    let left = 0
    let ans = 0
    const map = new Map()

    for (let right = 0; right < s.length; right++) {
        const char = s[right]
        map.set(char, (map.get(char) || 0) + 1)

        while (map.get(char) > 1) {
            const leftChar = s[left]
            map.set(leftChar, map.get(leftChar) - 1)
            left++
        }

        ans = Math.max(ans, right - left + 1)
    }

    return ans
};
```

## 6. 精华总结

滑动窗口判断口诀：

```txt
连续子数组 / 子字符串 + 最长 / 最短 / 满足条件
```

模板顺序：

```txt
right 进窗口
窗口不满足要求时调整 left
更新答案
```

一句话：

```txt
连续区间问题，优先想滑动窗口。
```
