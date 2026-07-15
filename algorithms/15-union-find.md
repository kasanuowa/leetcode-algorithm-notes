# 并查集 Union-Find

## 1. 核心思想
维护元素属于哪个集合，核心操作是 `find` 查根和 `union` 合并集合。

```js
function find(x) {
  if (parent[x] !== x) parent[x] = find(parent[x]);
  return parent[x];
}

function union(a, b) {
  const rootA = find(a);
  const rootB = find(b);
  if (rootA === rootB) return false;
  parent[rootA] = rootB;
  return true;
}
```

## 2. 适用题型
- 动态连通关系
- 连通分量数量
- 无向图检测环
- 好友圈、账户合并

## 3. 经典例题和逐步拆解
LeetCode 547：省份数量。

1. 每个城市初始自成集合。
2. 发现连接关系时合并两个根。
3. 只有合并两个不同集合时，集合数量减一。
4. 路径压缩和按秩合并用于优化。

复杂度：矩阵遍历 `O(n²)`，单次并查操作近似常数。

## 4. 易错点
- 直接写 `parent[a] = b`，没有连接根节点。
- `find` 只返回一层父节点。
- 重复合并时仍然减少集合数。
- 需要具体路径时误用并查集。

## 5. 今日练习题
LeetCode 684：冗余连接。若 `find(a) === find(b)`，当前边会形成环。

## 6. 精华总结
只关心“是否属于同一伙”时用并查集；需要距离或路径时用 DFS/BFS。