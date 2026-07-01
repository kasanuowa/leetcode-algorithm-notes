# LeetCode Algorithm Notes

这是一个用于系统学习 LeetCode 算法专题的笔记仓库。

当前已整理的专题：

1. 哈希表
2. 双指针
3. 滑动窗口
4. 二分查找
5. 栈
6. 综合练习课：Day 6 复盘

## 目录结构

```txt
leetcode-algorithm-notes/
├── README.md
├── algorithms/
│   ├── 01-hash-table.md
│   ├── 02-two-pointers.md
│   ├── 03-sliding-window.md
│   ├── 04-binary-search.md
│   └── 05-stack.md
└── practice/
    └── day-06-review.md
```

## 每个算法专题固定包含

1. 核心思想
2. 适用题型
3. 经典例题和逐步拆解
4. 易错点
5. 今日练习题
6. 精华总结

## 推荐学习路线

```txt
哈希表 → 双指针 → 滑动窗口 → 二分查找 → 栈 → 综合练习
```

后续可以继续追加：

```txt
队列/BFS、链表、树、递归、DFS、回溯、堆、贪心、动态规划、图论、前缀和、并查集
```

## GitHub 初始化建议

如果你已经创建了空仓库，可以在本地执行：

```bash
git init
git add .
git commit -m "init leetcode algorithm notes"
git branch -M main
git remote add origin <你的仓库地址>
git push -u origin main
```

如果你使用 GitHub CLI：

```bash
gh repo create leetcode-algorithm-notes --private --source=. --remote=origin --push
```
