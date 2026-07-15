# 10. 回溯 Backtracking

## 1. 核心思想

回溯用于枚举所有可能结果，固定动作是：

```txt
做选择 → 递归 → 撤销选择
```

常见变量：

```txt
path：当前已经选择的内容
result：最终答案集合
start / used：限制下一步可以选择什么
```

通用模板：

```js
const result = []
const path = []

function backtrack() {
    if (满足结束条件) {
        result.push([...path])
        return
    }

    for (const choice of choices) {
        path.push(choice)
        backtrack()
        path.pop()
    }
}
```

## 2. 适用题型

```txt
所有排列
所有组合
所有子集
所有可行路径
括号生成
单词搜索
N 皇后
```

典型 LeetCode：

```txt
22. Generate Parentheses
39. Combination Sum
46. Permutations
51. N-Queens
77. Combinations
78. Subsets
79. Word Search
```

## 3. 经典例题和逐步拆解

### LeetCode 46. Permutations

排列关心顺序，每个数字在同一条路径中只能使用一次，所以使用 `used` 数组。

```js
var permute = function(nums) {
    const result = []
    const path = []
    const used = new Array(nums.length).fill(false)

    const backtrack = () => {
        if (path.length === nums.length) {
            result.push([...path])
            return
        }

        for (let i = 0; i < nums.length; i++) {
            if (used[i]) continue

            path.push(nums[i])
            used[i] = true

            backtrack()

            path.pop()
            used[i] = false
        }
    }

    backtrack()
    return result
}
```

逐步理解：

```txt
path = [] 时选择第一个位置。
选择 1 后，used[0] = true，下一层不能再次选择 1。
得到完整排列后保存 path 的拷贝。
递归返回时撤销最后一个数字和 used 状态，再尝试其他分支。
```

复杂度：

```txt
时间复杂度：O(n * n!)
额外空间复杂度：O(n)，不含输出结果
```

## 4. 易错点

```txt
1. 忘记 path.pop()，后续分支继承了前一个分支的选择。
2. 忘记 used[i] = false，数字永久处于已使用状态。
3. result.push(path) 保存的是同一个数组引用，应使用 [...path]。
4. 排列每层从 0 开始并使用 used；组合和子集通常使用 start。
5. 子集题每个 path 都是答案，不一定等到固定长度才收集。
6. 去重题通常需要先排序，并在同一层跳过相同元素。
```

## 5. 今日练习题

### LeetCode 78. Subsets

```txt
难度：Medium
训练目标：start 参数、每个递归节点收集答案
```

提示：

```txt
进入 backtrack(start) 后先保存 [...path]。
循环从 start 开始。
选择 nums[i] 后递归 backtrack(i + 1)，保证后面只能继续向右选。
递归返回后 path.pop()。
```

模板：

```js
var subsets = function(nums) {
    const result = []
    const path = []

    const backtrack = (start) => {
        result.push([...path])

        for (let i = start; i < nums.length; i++) {
            path.push(nums[i])
            backtrack(i + 1)
            path.pop()
        }
    }

    backtrack(0)
    return result
}
```

## 6. 精华总结

```txt
回溯不是神秘算法，而是有结构地枚举所有选择。
push 和 pop 必须成对出现，used true 和 false 也必须成对出现。
排列使用 used，组合和子集常用 start。
保存答案时复制 path，不能保存同一个引用。
```
