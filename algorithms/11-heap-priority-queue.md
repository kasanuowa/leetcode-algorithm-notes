# 11. 堆与优先队列 Heap

## 1. 核心思想

堆用于动态维护当前最大值或最小值。

```txt
小顶堆：堆顶是最小值
大顶堆：堆顶是最大值
```

常见操作：

```txt
peek：查看堆顶，O(1)
push：插入元素并向上调整，O(log n)
pop：删除堆顶并向下调整，O(log n)
```

数组表示二叉堆时，下标关系为：

```js
const parent = Math.floor((i - 1) / 2)
const left = i * 2 + 1
const right = i * 2 + 2
```

## 2. 适用题型

```txt
第 K 大 / 第 K 小
Top K 高频元素
数据流中持续加入元素
反复获取当前最大值或最小值
合并多个有序序列
任务调度
```

典型 LeetCode：

```txt
23. Merge k Sorted Lists
215. Kth Largest Element in an Array
295. Find Median from Data Stream
347. Top K Frequent Elements
703. Kth Largest Element in a Stream
```

## 3. 经典例题和逐步拆解

### LeetCode 215. Kth Largest Element in an Array

维护一个大小为 `k` 的小顶堆：

```txt
堆中始终保存当前最大的 k 个数字。
当堆大小超过 k 时，弹出其中最小的数字。
最终堆顶就是第 k 大。
```

最小堆模板：

```js
class MinHeap {
    constructor() {
        this.heap = []
    }

    size() {
        return this.heap.length
    }

    peek() {
        return this.heap[0]
    }

    push(value) {
        this.heap.push(value)
        this.bubbleUp(this.heap.length - 1)
    }

    pop() {
        if (this.heap.length === 0) return undefined
        if (this.heap.length === 1) return this.heap.pop()

        const top = this.heap[0]
        this.heap[0] = this.heap.pop()
        this.bubbleDown(0)
        return top
    }

    bubbleUp(index) {
        while (index > 0) {
            const parent = Math.floor((index - 1) / 2)

            if (this.heap[parent] <= this.heap[index]) break

            ;[this.heap[parent], this.heap[index]] = [
                this.heap[index],
                this.heap[parent]
            ]
            index = parent
        }
    }

    bubbleDown(index) {
        const n = this.heap.length

        while (true) {
            let smallest = index
            const left = index * 2 + 1
            const right = index * 2 + 2

            if (left < n && this.heap[left] < this.heap[smallest]) {
                smallest = left
            }

            if (right < n && this.heap[right] < this.heap[smallest]) {
                smallest = right
            }

            if (smallest === index) break

            ;[this.heap[index], this.heap[smallest]] = [
                this.heap[smallest],
                this.heap[index]
            ]
            index = smallest
        }
    }
}
```

题目代码：

```js
var findKthLargest = function(nums, k) {
    const heap = new MinHeap()

    for (const num of nums) {
        heap.push(num)

        if (heap.size() > k) {
            heap.pop()
        }
    }

    return heap.peek()
}
```

复杂度：

```txt
时间复杂度：O(n log k)
空间复杂度：O(k)
```

## 4. 易错点

```txt
1. 第 K 大通常使用大小为 K 的小顶堆，而不是大顶堆。
2. 堆只保证堆顶最小或最大，不保证整个数组有序。
3. push 后要 bubbleUp，pop 后要 bubbleDown。
4. bubbleDown 必须同时比较左右孩子。
5. pop 时要处理空堆和只有一个元素的情况。
6. 堆中存 [value, freq] 时，比较逻辑应基于 freq，而不是整个数组。
```

## 5. 今日练习题

### LeetCode 703. Kth Largest Element in a Stream

```txt
难度：Easy
训练目标：构造函数初始化堆、add 动态维护前 K 大
```

提示：

```txt
构造函数保存 k，并把 nums 中每个数依次交给 add。
add(val) 把 val 入堆；如果 size > k 就弹出堆顶；返回 peek()。
除了 add，size、peek、push、pop、bubbleUp、bubbleDown 都可以作为类方法。
```

## 6. 精华总结

```txt
完整排序用 sort；只关心前 K 个时优先考虑堆。
第 K 大：维护大小为 K 的小顶堆。
第 K 小：维护大小为 K 的大顶堆。
堆的核心不是背数组，而是理解向上调整和向下调整恢复堆序。
```
