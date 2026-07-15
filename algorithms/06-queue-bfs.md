# 06. 队列与 BFS

## 1. 核心思想

队列遵循先进先出，BFS 使用队列按距离或层级向外扩展。

```txt
先进入队列的节点先处理
处理当前节点时，把下一层节点加入队尾
```

JavaScript 中不建议频繁使用 `shift()`，可以使用 `head` 指针：

```js
const queue = [start]
let head = 0

while (head < queue.length) {
    const current = queue[head]
    head++
}
```

需要区分层级时，先固定当前层大小：

```js
const levelSize = queue.length - head
```

## 2. 适用题型

```txt
二叉树层序遍历
最短步数 / 最少分钟
网格扩散、感染、腐烂
无权图最短路径
从多个起点同时扩散
```

典型 LeetCode：

```txt
102. Binary Tree Level Order Traversal
199. Binary Tree Right Side View
994. Rotting Oranges
1091. Shortest Path in Binary Matrix
127. Word Ladder
```

## 3. 经典例题和逐步拆解

### LeetCode 102. Binary Tree Level Order Traversal

目标：按层返回二叉树节点值。

```js
var levelOrder = function(root) {
    if (!root) return []

    const result = []
    const queue = [root]
    let head = 0

    while (head < queue.length) {
        const levelSize = queue.length - head
        const level = []

        for (let i = 0; i < levelSize; i++) {
            const node = queue[head]
            head++

            level.push(node.val)

            if (node.left) queue.push(node.left)
            if (node.right) queue.push(node.right)
        }

        result.push(level)
    }

    return result
}
```

逐步理解：

```txt
1. queue 中保存尚未处理的节点。
2. 每轮开始时，queue.length - head 就是当前层节点数。
3. 当前层处理中加入的新节点属于下一层，不能在本轮继续处理。
4. 当前层处理完后，再把 level 放入 result。
```

复杂度：

```txt
时间复杂度：O(n)
空间复杂度：O(n)
```

## 4. 易错点

```txt
1. 忘记 root 为空时直接返回 []。
2. 层序题没有提前保存 levelSize，导致下一层被混进当前层。
3. 网格 BFS 入队时存的是坐标 [r, c]，不是格子值。
4. 图或网格中忘记 visited，造成重复访问。
5. 最短时间题通常每处理完一层才 minutes++。
6. 多源 BFS 要把所有起点一开始同时入队。
```

## 5. 今日练习题

### LeetCode 994. Rotting Oranges

```txt
难度：Medium
训练目标：多源 BFS、按层统计分钟数
```

提示：

```txt
先把所有腐烂橘子加入队列，并统计新鲜橘子 fresh。
每一层代表一分钟，感染新橘子时立刻改为 2、fresh-- 并入队。
最后 fresh === 0 才能返回分钟数，否则返回 -1。
```

## 6. 精华总结

```txt
按层处理或寻找无权图最短步数，用 BFS。
普通遍历：queue + head。
需要层级：额外保存 levelSize。
需要避免重复：入队时就标记 visited。
```
