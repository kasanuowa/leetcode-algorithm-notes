# 06. 队列 / BFS Queue & Breadth-First Search

## 1. 核心思想

队列的特点是：

```txt
先进先出，First In First Out
```

BFS，也就是广度优先搜索，核心是：

```txt
从起点开始，一层一层向外扩散。
```

它和栈/DFS 的区别很明显：

```txt
栈 / DFS：先往深处走
队列 / BFS：先把当前层处理完，再处理下一层
```

在 JavaScript 里，可以用数组配合 `head` 指针模拟队列，避免频繁使用 `shift()`：

```js
const queue = [];
let head = 0;

queue.push(start);

while (head < queue.length) {
  const cur = queue[head];
  head++;

  // 处理 cur
  // 把下一层节点加入 queue
}
```

BFS 最常见的意义是：

```txt
每一轮扩散一步，所以天然适合求最短步数、最少分钟数、层序遍历。
```

---

## 2. 适用题型

看到这些关键词，优先考虑 BFS：

```txt
层序遍历
一层一层处理
最短路径
最少步数
从一个点向四周扩散
网格感染 / 腐烂 / 扩散
图的广度搜索
```

典型题目：

```txt
LeetCode 102：Binary Tree Level Order Traversal
LeetCode 199：Binary Tree Right Side View
LeetCode 994：Rotting Oranges
LeetCode 200：Number of Islands
LeetCode 127：Word Ladder
```

常见搭配：

```txt
树的 BFS：一般不需要 visited
图 / 网格 BFS：通常需要 visited，或者直接修改原数组标记已访问
```

---

## 3. 经典例题和逐步拆解

### 经典例题：LeetCode 102. Binary Tree Level Order Traversal

题意：

给定一棵二叉树，返回它的层序遍历结果。

示例：

```txt
        3
       / \
      9   20
          / \
         15  7
```

输出：

```txt
[
  [3],
  [9, 20],
  [15, 7]
]
```

### 为什么用 BFS？

题目要求按层输出：

```txt
第 1 层 -> 第 2 层 -> 第 3 层
```

队列可以保证：

```txt
先入队的上一层节点先处理；
处理上一层时，再把下一层节点放进队列。
```

所以 BFS 天然就是层序遍历。

### JavaScript 代码

```js
function levelOrder(root) {
  if (!root) return [];

  const result = [];
  const queue = [root];
  let head = 0;

  while (head < queue.length) {
    const levelSize = queue.length - head;
    const level = [];

    for (let i = 0; i < levelSize; i++) {
      const node = queue[head];
      head++;

      level.push(node.val);

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }

    result.push(level);
  }

  return result;
}
```

### 逐步拆解

初始：

```txt
queue = [3]
head = 0
result = []
```

处理第 1 层：

```txt
levelSize = 1
处理节点 3
把 9 和 20 入队
result = [[3]]
```

队列状态：

```txt
queue = [3, 9, 20]
head = 1
未处理部分 = [9, 20]
```

处理第 2 层：

```txt
levelSize = 2
处理 9 和 20
20 的孩子 15、7 入队
result = [[3], [9, 20]]
```

处理第 3 层：

```txt
levelSize = 2
处理 15 和 7
没有新节点入队
result = [[3], [9, 20], [15, 7]]
```

返回结果。

### 复杂度分析

```txt
时间复杂度：O(n)
空间复杂度：O(n)
```

每个节点只会入队、出队一次。最坏情况下，队列可能存下一整层节点。

---

## 4. 易错点

### 易错点 1：忘记空树判断

```js
if (!root) return [];
```

否则后面访问 `node.val` 时会报错。

### 易错点 2：没有固定当前层长度

这句很关键：

```js
const levelSize = queue.length - head;
```

因为处理当前层时会不断把下一层节点加入队列。如果不先固定当前层长度，很容易把下一层也提前处理掉，导致层级混乱。

### 易错点 3：JS 中无脑 `shift()`

```js
queue.shift();
```

写起来方便，但数组大时不够好。更推荐：

```js
const node = queue[head];
head++;
```

### 易错点 4：图 / 网格 BFS 忘记 visited

树没有回边，一般不用 `visited`。但图和网格里很容易重复访问：

```js
const visited = new Set();
visited.add(`${row},${col}`);
```

也可以直接把原 grid 中访问过的位置改掉。

---

## 5. 今日练习题

### LeetCode 994：Rotting Oranges

题意：

给一个网格：

```txt
0 表示空格
1 表示新鲜橘子
2 表示腐烂橘子
```

每过 1 分钟，腐烂橘子会让上下左右相邻的新鲜橘子腐烂。问最少多少分钟后，所有新鲜橘子都会腐烂；如果不可能，返回 `-1`。

训练目标：

```txt
多源 BFS
按层统计分钟数
网格四方向扩散
```

提示：

```txt
1. 先遍历 grid：
   - 把所有腐烂橘子加入队列
   - 统计新鲜橘子数量 fresh

2. BFS 按层扩散：
   - 每一层代表 1 分钟
   - 感染一个新鲜橘子，fresh--

3. 最后：
   - fresh === 0 返回 minutes
   - 否则返回 -1
```

方向数组：

```js
const dirs = [
  [1, 0],
  [-1, 0],
  [0, 1],
  [0, -1],
];
```

代码骨架：

```js
function orangesRotting(grid) {
  const rows = grid.length;
  const cols = grid[0].length;
  const queue = [];
  let head = 0;
  let fresh = 0;
  let minutes = 0;

  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      if (grid[r][c] === 2) {
        queue.push([r, c]);
      } else if (grid[r][c] === 1) {
        fresh++;
      }
    }
  }

  const dirs = [
    [1, 0],
    [-1, 0],
    [0, 1],
    [0, -1],
  ];

  while (head < queue.length && fresh > 0) {
    const levelSize = queue.length - head;

    for (let i = 0; i < levelSize; i++) {
      const [r, c] = queue[head];
      head++;

      for (const [dr, dc] of dirs) {
        const nr = r + dr;
        const nc = c + dc;

        if (
          nr < 0 || nr >= rows ||
          nc < 0 || nc >= cols ||
          grid[nr][nc] !== 1
        ) {
          continue;
        }

        grid[nr][nc] = 2;
        fresh--;
        queue.push([nr, nc]);
      }
    }

    minutes++;
  }

  return fresh === 0 ? minutes : -1;
}
```

---

## 6. 精华总结

BFS 一句话：

```txt
用队列维护下一批要处理的节点，一层一层向外扩散。
```

核心模板：

```js
const queue = [start];
let head = 0;

while (head < queue.length) {
  const cur = queue[head];
  head++;

  // 处理 cur
  // 把下一层加入 queue
}
```

按层处理时，必须固定当前层长度：

```js
const levelSize = queue.length - head;
```

题型判断：

```txt
最近未匹配 -> 栈
一层一层扩散 -> 队列 / BFS
连续区间最长最短 -> 滑动窗口
有序查位置 -> 二分
快速查出现过 -> 哈希表
```
