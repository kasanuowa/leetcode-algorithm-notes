# 链表

## 1. 核心思想
链表题本质是修改节点之间的 `next` 指向。改指针前先保存后续节点。

```js
let prev = null;
let cur = head;
while (cur) {
  const next = cur.next;
  cur.next = prev;
  prev = cur;
  cur = next;
}
return prev;
```

## 2. 适用题型
- 反转、合并、删除节点
- 找中点、倒数第 N 个节点
- 判断环、链表相交

## 3. 经典例题和逐步拆解
LeetCode 206：反转链表。

1. `prev` 表示已反转部分。
2. `cur` 表示当前节点。
3. 先保存 `next = cur.next`。
4. 改为 `cur.next = prev`。
5. 同步移动 `prev` 和 `cur`。

复杂度：时间 `O(n)`，空间 `O(1)`。

## 4. 易错点
- 没保存 `next` 就改指针，导致后续链表丢失。
- 反转后返回 `head`，正确答案是 `prev`。
- 删除头节点时不使用 `dummy`，边界处理变复杂。
- 合并链表后忘记移动 `cur`。

## 5. 今日练习题
LeetCode 21：合并两个有序链表。练习 `dummy` 和尾指针。

## 6. 精华总结
链表题先画图；保存、断开、连接、移动，顺序不能乱。