# 09. 网格 DFS

## 1. 核心思想

网格 DFS 从一个格子出发，沿上下左右不断访问与它连通的格子。

固定流程：

```txt
1. 判断坐标是否越界
2. 判断当前格子是否符合访问条件
3. 立刻标记当前格子已访问
4. 递归访问上下左右
```

方向数组：

```js
const dirs = [
    [1, 0],
    [-1, 0],
    [0, 1],
    [0, -1]
]
```

## 2. 适用题型

```txt
岛屿数量
连通区域面积
洪水填充 / 染色
矩阵中寻找完整区域
四方向可达性
```

典型 LeetCode：

```txt
130. Surrounded Regions
200. Number of Islands
417. Pacific Atlantic Water Flow
695. Max Area of Island
733. Flood Fill
```

## 3. 经典例题和逐步拆解

### LeetCode 200. Number of Islands

遇到一个尚未访问的陆地 `"1"`，说明发现一个新岛屿：

```txt
count++
然后用 DFS 把整座岛全部标记掉
```

```js
var numIslands = function(grid) {
    const rows = grid.length
    const cols = grid[0].length
    let count = 0

    const dfs = (r, c) => {
        if (
            r < 0 ||
            r >= rows ||
            c < 0 ||
            c >= cols ||
            grid[r][c] !== "1"
        ) {
            return
        }

        grid[r][c] = "0"

        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)
    }

    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (grid[r][c] === "1") {
                count++
                dfs(r, c)
            }
        }
    }

    return count
}
```

逐步理解：

```txt
外层双循环负责寻找每座岛的第一个格子。
DFS 负责一次性清除与该格子连通的所有陆地。
同一座岛的其他格子被改成 "0" 后，不会再次增加 count。
```

复杂度：

```txt
时间复杂度：O(m * n)
空间复杂度：最坏 O(m * n)，来自递归栈
```

## 4. 易错点

```txt
1. 边界必须使用 r >= 0、r < rows、c >= 0、c < cols。
2. LeetCode 200 使用字符串 "1" / "0"，不是数字 1 / 0。
3. 必须先标记当前格子，再递归邻居，否则相邻格子可能反复调用。
4. 队列或手动栈中要保存坐标 [r, c]，不能只保存格子值。
5. Flood Fill 中 origin === color 时要提前返回，否则会重复访问。
6. 数据规模很大时，JavaScript 递归可能爆栈，可改用手动栈或 BFS。
```

## 5. 今日练习题

### LeetCode 733. Flood Fill

```txt
难度：Easy
训练目标：保存原颜色、四方向 DFS、原地标记
```

提示：

```txt
origin = image[sr][sc]。
如果 origin === color，直接返回。
只有 image[r][c] === origin 时才能继续。
进入合法格子后立刻改成 color，再递归四个方向。
```

## 6. 精华总结

```txt
二维网格中寻找一整片连通区域，优先考虑 DFS / BFS。
DFS 模板永远先处理越界和不合法，再标记，再扩散。
外层循环负责发现新的连通块，内部 DFS 负责清理整个连通块。
```
