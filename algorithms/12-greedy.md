# 12. 贪心 Greedy

## 1. 核心思想

贪心算法在每一步选择当前最有利的方案，并要求这个局部最优不会破坏全局最优。

做贪心题要回答：

```txt
1. 每一步在贪什么？
2. 为什么当前选择不会让未来更差？
3. 是否需要先排序，让选择顺序稳定？
```

常见策略：

```txt
优先选择最小代价
优先选择结束最早的区间
维护当前最远覆盖范围
用最小资源满足最小需求
```

## 2. 适用题型

```txt
最多完成多少任务
最少操作次数
区间选择和区间合并
跳跃覆盖范围
分配资源
股票单次或多次收益
```

典型 LeetCode：

```txt
45. Jump Game II
55. Jump Game
121. Best Time to Buy and Sell Stock
435. Non-overlapping Intervals
452. Minimum Number of Arrows to Burst Balloons
455. Assign Cookies
```

## 3. 经典例题和逐步拆解

### LeetCode 455. Assign Cookies

策略：

```txt
把孩子胃口和饼干尺寸都升序排序。
用当前最小且能满足孩子的饼干，满足当前胃口最小的孩子。
```

```js
var findContentChildren = function(g, s) {
    g.sort((a, b) => a - b)
    s.sort((a, b) => a - b)

    let child = 0
    let cookie = 0

    while (child < g.length && cookie < s.length) {
        if (s[cookie] >= g[child]) {
            child++
            cookie++
        } else {
            cookie++
        }
    }

    return child
}
```

为什么正确：

```txt
如果当前最小饼干不能满足胃口最小的孩子，它也不能满足后面的孩子，可以直接跳过。
如果能满足，就用这块最小饼干，避免浪费更大的饼干。
```

复杂度：

```txt
时间复杂度：O(n log n + m log m)
额外空间复杂度：取决于排序实现
```

## 4. 易错点

```txt
1. 没排序就直接执行双指针策略，无法保证局部选择安全。
2. 饼干太小时应该移动饼干指针，不能跳过仍未满足的孩子。
3. 满足孩子后，孩子和饼干两个指针都要移动。
4. 看到最大或最小就无脑贪心；很多题实际上需要 DP。
5. 贪心方案至少要能解释为什么不会让未来更差。
6. 有些贪心题维护的是覆盖范围，不需要真的模拟每一步选择。
```

## 5. 今日练习题

### LeetCode 55. Jump Game

```txt
难度：Medium
训练目标：维护当前最远可达下标
```

提示：

```txt
farthest 表示当前能够覆盖到的最远下标。
遍历位置 i 时，如果 i > farthest，说明当前位置无法到达，返回 false。
否则更新 farthest = Math.max(farthest, i + nums[i])。
不需要记录具体怎么跳，只关心覆盖范围。
```

模板：

```js
var canJump = function(nums) {
    let farthest = 0

    for (let i = 0; i < nums.length; i++) {
        if (i > farthest) return false

        farthest = Math.max(farthest, i + nums[i])

        if (farthest >= nums.length - 1) return true
    }

    return true
}
```

## 6. 精华总结

```txt
贪心不是“看起来最划算”，而是局部最优能够保证全局最优。
常见信号：最少、最多、覆盖范围、区间选择、资源分配。
说不清为什么当前选择安全时，要警惕题目可能需要动态规划。
```
