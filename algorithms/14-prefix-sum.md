# 前缀和

## 1. 核心思想
提前保存累计和，使任意连续区间求和由两个前缀和相减得到。

```js
const prefix = new Array(nums.length + 1).fill(0);
for (let i = 0; i < nums.length; i++) {
  prefix[i + 1] = prefix[i] + nums[i];
}
// [left, right]
const sum = prefix[right + 1] - prefix[left];
```

## 2. 适用题型
- 多次区间和查询
- 左右两侧和
- 连续子数组和等于目标
- 二维矩阵区域和

## 3. 经典例题和逐步拆解
LeetCode 724：寻找数组中心下标。

1. 先计算总和 `total`。
2. 动态维护当前位置左侧和 `leftSum`。
3. 右侧和为 `total - leftSum - nums[i]`。
4. 先判断，再把当前元素加入 `leftSum`。

复杂度：时间 `O(n)`，空间 `O(1)`。

## 4. 易错点
- 判断前就把当前元素算进左侧。
- 区间公式漏掉 `right + 1`。
- 忘记子数组必须连续。
- 数组含负数时误用普通滑动窗口。

## 5. 今日练习题
LeetCode 560：和为 K 的子数组。使用前缀和 + 哈希表，并初始化 `map.set(0, 1)`。

## 6. 精华总结
连续子数组之和等于两个位置的前缀和之差。