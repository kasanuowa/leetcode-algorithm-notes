# 08. 二叉树递归 DFS

## 1. 核心思想

二叉树递归通常把问题拆成：

```txt
当前节点
左子树
右子树
```

写递归前先回答三个问题：

```txt
1. dfs(root) 返回什么？
2. root 为空时返回什么？
3. 左右子树结果如何合并成当前节点答案？
```

通用模板：

```js
function dfs(root) {
    if (root === null) return baseValue

    const left = dfs(root.left)
    const right = dfs(root.right)

    return combine(root, left, right)
}
```

## 2. 适用题型

```txt
最大或最小深度
翻转二叉树
判断两棵树是否相同
路径和
判断平衡或对称
统计节点数量
```

典型 LeetCode：

```txt
100. Same Tree
101. Symmetric Tree
104. Maximum Depth of Binary Tree
110. Balanced Binary Tree
112. Path Sum
226. Invert Binary Tree
```

## 3. 经典例题和逐步拆解

### LeetCode 104. Maximum Depth of Binary Tree

定义：

```txt
maxDepth(root) 返回以 root 为根的最大深度。
```

代码：

```js
var maxDepth = function(root) {
    if (root === null) return 0

    const leftDepth = maxDepth(root.left)
    const rightDepth = maxDepth(root.right)

    return Math.max(leftDepth, rightDepth) + 1
}
```

逐步理解：

```txt
叶子节点左右孩子都是 null。
null 的深度是 0，所以叶子深度为 max(0, 0) + 1 = 1。
父节点拿到左右子树深度后，选择更大的一个，再加当前节点这一层。
```

复杂度：

```txt
时间复杂度：O(n)，每个节点访问一次
空间复杂度：O(h)，h 为树高
平衡树约 O(log n)，退化链表最坏 O(n)
```

## 4. 易错点

```txt
1. 没定义清楚递归函数返回值，导致层数、路径和等含义混乱。
2. 空节点返回值与函数定义不匹配。
3. 最大深度忘记加当前节点这一层的 +1。
4. 用外部变量记录深度时，递归返回值和参数 d 混在一起。
5. 翻转树时保存或交换顺序混乱，建议明确使用递归返回的左右子树。
6. DFS 和 BFS 都能遍历树，但按层输出更适合 BFS。
```

## 5. 今日练习题

### LeetCode 226. Invert Binary Tree

```txt
难度：Easy
训练目标：递归返回新子树、交换左右孩子
```

提示：

```txt
空节点返回 null。
递归得到翻转后的 left 和 right。
让 root.left = right，root.right = left。
最后返回 root。
```

模板：

```js
var invertTree = function(root) {
    if (root === null) return null

    const left = invertTree(root.left)
    const right = invertTree(root.right)

    root.left = right
    root.right = left

    return root
}
```

## 6. 精华总结

```txt
树递归的第一步不是写代码，而是定义 dfs 的返回值。
空节点返回什么，由 dfs 的含义决定。
当前节点的答案通常由左子树答案、右子树答案和当前节点共同组成。
看到“左右子树结果能推出当前答案”，优先考虑递归 DFS。
```
