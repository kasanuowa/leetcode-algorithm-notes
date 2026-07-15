# LeetCode Algorithm Notes

这是一个用于系统学习和复习 LeetCode 算法专题的 JavaScript 笔记仓库。

## 已完成课程

20 节课程包含 17 个算法专题和 3 次综合复盘：

1. 哈希表
2. 双指针
3. 滑动窗口
4. 二分查找
5. 普通栈
6. 综合练习：Day 6
7. 队列 / BFS
8. 链表
9. 二叉树递归 DFS
10. 网格 DFS
11. 回溯
12. 综合练习：Day 12
13. 堆 / 优先队列
14. 贪心
15. 动态规划入门
16. 前缀和
17. 并查集
18. 综合练习：Day 18
19. 拓扑排序
20. 单调栈

## 目录结构

```txt
leetcode-algorithm-notes/
├── README.md
├── algorithms/
│   ├── 01-hash-table.md
│   ├── 02-two-pointers.md
│   ├── 03-sliding-window.md
│   ├── 04-binary-search.md
│   ├── 05-stack.md
│   ├── 06-queue-bfs.md
│   ├── 07-linked-list.md
│   ├── 08-binary-tree-dfs.md
│   ├── 09-grid-dfs.md
│   ├── 10-backtracking.md
│   ├── 11-heap-priority-queue.md
│   ├── 12-greedy.md
│   ├── 13-dynamic-programming.md
│   ├── 14-prefix-sum.md
│   ├── 15-union-find.md
│   ├── 16-topological-sort.md
│   └── 17-monotonic-stack.md
└── practice/
    ├── day-06-review.md
    ├── day-12-review.md
    └── day-18-review.md
```

## 每个算法专题固定包含

1. 核心思想
2. 适用题型
3. 经典例题和逐步拆解
4. 易错点
5. 今日练习题
6. 精华总结

## 已掌握的题型判断树

```txt
快速查出现过                         → 哈希表
左右收缩 / 原地整理                  → 双指针
连续区间最长最短                     → 滑动窗口
有序数组查位置                       → 二分查找
最近出现但未匹配                     → 普通栈
一层一层扩散 / 最少步数              → BFS
修改 next 指向                       → 链表
左右子树合并答案                     → 二叉树递归 DFS
二维连通区域                         → 网格 DFS
枚举所有可能结果                     → 回溯
动态维护最大值 / 最小值 / Top K       → 堆
每一步局部最优                       → 贪心
当前答案依赖更小状态                 → 动态规划
连续子数组和 / 区间和                → 前缀和
动态维护集合连通性                   → 并查集
有向依赖与执行顺序                   → 拓扑排序
某方向第一个更大或更小元素           → 单调栈
```

## 复习阶段建议

每天轮换复习 3 个专题，每个专题完成 1 道 Easy 或 Medium：

- 先独立判断题型，不急着看提示。
- 写代码前说清楚核心变量的含义。
- 卡住时只看一级提示，再逐层展开。
- 记录错误原因，而不只保存正确代码。
- 同一道题至少间隔 7 天再重复。

当前目标不是继续扩大知识点，而是把这些模板练到能够独立识别、独立实现、独立处理边界。