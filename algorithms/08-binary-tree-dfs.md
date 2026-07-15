# 二叉树递归 DFS

## 1. 核心思想
把大问题拆成“当前节点 + 左子树 + 右子树”。写递归前先定义函数返回值。

```js
function dfs(root) {
  if (!root) return baseValue;
  const left = dfs(root.left);
  const right = dfs(root.right);
  return combine(root, left, right);
}
```

## 2. 适用题型
- 最大/最小深度
- 翻转、相同树、对称树
- 路径和、节点统计、平衡判断

## 3. 经典例题和逐步拆解
LeetCode 104：二叉树最大深度。

定义 `maxDepth(root)` 为以 `root` 为根的最大深度。空节点返回 `0`；当前深度等于左右子树较大深度加一。

```js
function maxDepth(root) {
  if (!root) return 0;
  return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

## 4. 易错点
- 没定义 `dfs` 返回什么。
- 空节点返回值与函数语义不匹配。
- 忘记把当前节点这一层 `+1`。
- 把 BFS 的层序逻辑硬塞进递归 DFS。

## 5. 今日练习题
LeetCode 226：翻转二叉树。练习递归返回翻转后的根节点。

## 6. 精华总结
树递归三问：空节点返回什么、左右子树给我什么、当前节点如何合并。