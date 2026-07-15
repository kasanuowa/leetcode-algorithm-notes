# 13. 动态规划 Dynamic Programming

## 1. 核心思想

动态规划把大问题拆成重复的小问题，并保存小问题答案，避免重复计算。

写 DP 前先回答三件事：

```txt
1. dp[i] 表示什么？
2. dp[i] 可以从哪些前置状态转移而来？
3. 初始状态是什么？
```

常见结构：

```txt
方法数量：通常使用加法
最大收益：通常使用 Math.max
最小代价：通常使用 Math.min
能否到达：通常使用 boolean
```

## 2. 适用题型

```txt
有多少种方法
最大或最小收益
选或不选
当前答案依赖前面若干状态
背包、子序列、路径计数
```

典型 LeetCode：

```txt
70. Climbing Stairs
198. House Robber
300. Longest Increasing Subsequence
322. Coin Change
746. Min Cost Climbing Stairs
1143. Longest Common Subsequence
```

## 3. 经典例题和逐步拆解

### LeetCode 70. Climbing Stairs

定义：

```txt
dp[i] = 到达第 i 阶的方法数量
```

到达第 `i` 阶的最后一步只有两种：

```txt
从 i - 1 阶走 1 步
从 i - 2 阶走 2 步
```

因此：

```js
dp[i] = dp[i - 1] + dp[i - 2]
```

代码：

```js
var climbStairs = function(n) {
    if (n <= 2) return n

    const dp = new Array(n + 1).fill(0)
    dp[1] = 1
    dp[2] = 2

    for (let i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2]
    }

    return dp[n]
}
```

空间优化：

```js
var climbStairs = function(n) {
    if (n <= 2) return n

    let prev2 = 1
    let prev1 = 2

    for (let i = 3; i <= n; i++) {
        const current = prev1 + prev2
        prev2 = prev1
        prev1 = current
    }

    return prev1
}
```

复杂度：

```txt
时间复杂度：O(n)
空间复杂度：数组版 O(n)，优化后 O(1)
```

## 4. 易错点

```txt
1. 没定义 dp[i] 就开始写转移方程，变量含义会逐渐失控。
2. 初始化与 dp 定义不匹配，后面的状态全部跟着错。
3. 循环起点没有避开已经初始化的位置。
4. 方法数问题误用 Math.max，最优值问题误用加法。
5. 空间优化时更新顺序错误，旧状态被提前覆盖。
6. 贪心与 DP 混淆：存在多个选择并需要比较前置结果时，通常更偏 DP。
```

## 5. 今日练习题

### LeetCode 198. House Robber

```txt
难度：Medium
训练目标：选或不选、最大收益状态转移
```

定义：

```txt
dp[i] = 考虑到第 i 间房子时能获得的最大金额
```

转移：

```txt
不偷第 i 间：dp[i - 1]
偷第 i 间：dp[i - 2] + nums[i]
```

```js
var rob = function(nums) {
    if (nums.length === 0) return 0
    if (nums.length === 1) return nums[0]

    const dp = new Array(nums.length).fill(0)
    dp[0] = nums[0]
    dp[1] = Math.max(nums[0], nums[1])

    for (let i = 2; i < nums.length; i++) {
        dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i])
    }

    return dp[nums.length - 1]
}
```

## 6. 精华总结

```txt
DP 三件套：状态定义、状态转移、初始化。
先用一句完整的话定义 dp[i]，再写公式。
当前状态依赖前面状态，并且存在重复子问题时，优先考虑动态规划。
先掌握一维 DP，再进入背包和二维 DP，别急着一口吞大象。
```
