# 07. 链表 Linked List

## 1. 核心思想

链表不能像数组一样通过下标随机访问，只能沿着 `next` 逐个向后移动。

链表题的本质通常是修改节点之间的连接关系：

```txt
当前节点 cur
前一个节点 prev
下一个节点 next
```

改动 `cur.next` 前，必须先保存原来的后续节点，否则后面的链表可能直接丢失。

常见辅助技巧：

```txt
dummy：处理头节点可能变化的情况
快慢指针：中点、环、倒数第 N 个节点
prev / cur / next：反转链表
```

## 2. 适用题型

```txt
反转链表
合并有序链表
删除指定或倒数节点
寻找链表中点
判断是否存在环
寻找两个链表交点
```

典型 LeetCode：

```txt
19. Remove Nth Node From End of List
21. Merge Two Sorted Lists
141. Linked List Cycle
160. Intersection of Two Linked Lists
206. Reverse Linked List
876. Middle of the Linked List
```

## 3. 经典例题和逐步拆解

### LeetCode 206. Reverse Linked List

```js
var reverseList = function(head) {
    let prev = null
    let cur = head

    while (cur !== null) {
        const next = cur.next
        cur.next = prev
        prev = cur
        cur = next
    }

    return prev
}
```

逐步理解：

```txt
原链表：1 -> 2 -> 3 -> null

第 1 轮：
next 保存 2
把 1 指向 null
prev 移到 1，cur 移到 2

第 2 轮：
next 保存 3
把 2 指向 1
prev 移到 2，cur 移到 3

循环结束时：
prev 指向新头节点 3
cur 为 null
```

为什么返回 `prev`：原来的 `head` 反转后成为尾节点，新头节点是最后处理的节点。

复杂度：

```txt
时间复杂度：O(n)
空间复杂度：O(1)
```

## 4. 易错点

```txt
1. 改 cur.next 前没有先保存 next，导致后续节点丢失。
2. 反转完成后返回 head，而不是新的头节点 prev。
3. 使用 const 声明 prev 或 cur，后续却需要重新赋值。
4. dummy 是虚拟头节点，最终通常返回 dummy.next。
5. 合并链表时接上节点后，cur 和对应链表指针都要向后移动。
6. 快慢指针题要先明确两者的起点和间隔。
```

## 5. 今日练习题

### LeetCode 21. Merge Two Sorted Lists

```txt
难度：Easy
训练目标：dummy 节点、双链表指针、拼接 next
```

提示：

```txt
创建 dummy 和 cur。
比较 list1.val 与 list2.val，谁小就接到 cur.next。
被使用的链表向后走一步，cur 也向后走一步。
循环结束后把剩余链表直接接上。
返回 dummy.next。
```

模板：

```js
var mergeTwoLists = function(list1, list2) {
    const dummy = new ListNode(0)
    let cur = dummy

    while (list1 && list2) {
        if (list1.val <= list2.val) {
            cur.next = list1
            list1 = list1.next
        } else {
            cur.next = list2
            list2 = list2.next
        }

        cur = cur.next
    }

    cur.next = list1 || list2
    return dummy.next
}
```

## 6. 精华总结

```txt
修改 next 前先保存后续节点。
头节点可能变化时优先考虑 dummy。
反转链表固定顺序：保存 next → 改指向 → 移动 prev → 移动 cur。
链表题不要只在脑中想，画出节点和箭头更稳。
```
