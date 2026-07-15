# 队列 / BFS

## 1. 核心思想
队列先进先出；BFS 按层扩散。适合最短步数、层序遍历和网格传播。

```js
const queue = [start];
let head = 0;
while (head < queue.length) {
  const levelSize = queue.length - head;
  for (let i = 0; i < levelSize; i++) {
    const cur = queue[head++];
    // 处理 cur，并把下一层加入队列
  }
}
```

## 2. 适用题型
- 二叉树层序遍历
- 网格扩散、感染问题
- 无权图最短路径、最少步数

## 3. 经典例题和逐步拆解
LeetCode 102：二叉树层序遍历。

1. 根节点入队。
2. 每轮固定 `levelSize`，只处理当前层。
3. 当前节点的左右孩子入队，留到下一轮处理。
4. 每层收集成独立数组。

复杂度：时间 `O(n)`，空间 `O(n)`。

## 4. 易错点
- 忘记空树判断。
- 不固定 `levelSize`，导致下一层混入当前层。
- JS 中大量使用 `shift()`；更推荐 `head` 指针。
- 图和网格题忘记 `visited` 或访问标记。

## 5. 今日练习题
LeetCode 994：腐烂的橘子。练习多源 BFS、按层统计分钟数。

## 6. 精华总结
看到“一层一层”“最少步数”“同时扩散”，优先考虑 BFS。