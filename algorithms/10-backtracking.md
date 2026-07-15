# 回溯

## 1. 核心思想
回溯是结构化枚举：做选择、递归、撤销选择。

```js
function backtrack(start) {
  result.push([...path]);
  for (let i = start; i < nums.length; i++) {
    path.push(nums[i]);
    backtrack(i + 1);
    path.pop();
  }
}
```

## 2. 适用题型
- 所有排列、组合、子集
- 括号生成、棋盘搜索、N 皇后
- 所有可行路径

## 3. 经典例题和逐步拆解
LeetCode 46：全排列。

1. `path` 保存当前排列。
2. `used[i]` 表示当前路径是否已经使用该元素。
3. 路径长度等于数组长度时保存副本。
4. 递归返回后同时恢复 `path` 和 `used`。

复杂度：时间约 `O(n·n!)`，额外空间 `O(n)`。

## 4. 易错点
- 忘记 `path.pop()` 或 `used[i] = false`。
- 使用 `result.push(path)`，导致保存同一个数组引用。
- 排列用 `used`，组合/子集通常用 `start`，不要混用。
- 收集答案的时机写错。

## 5. 今日练习题
LeetCode 78：子集。每个递归节点对应一个合法答案。

## 6. 精华总结
回溯的灵魂不是递归，而是递归回来后必须恢复现场。