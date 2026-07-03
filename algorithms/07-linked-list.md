# 07. 链表 Linked List

## 1. 核心思想

链表和数组最大的区别是：

```txt
数组可以通过下标随机访问；
链表只能通过 next 一步一步往后走。
```

单链表节点通常长这样：

```js
function ListNode(val, next = null) {
  this.val = val;
  this.next = next;
}
```

例如：

```txt
1 -> 2 -> 3 -> null
```

每个节点只知道自己的下一个节点。

所以链表题的核心不是下标，而是：

```txt
节点在哪里？
next 指向谁？
改 next 前有没有先保存后面的链表？
```

最常见的几个指针：

```txt
prev：前一个节点
cur：当前节点
next：提前保存的下一个节点
slow / fast：快慢指针
```

链表题第一原则：

```txt
先保存，再断开，再连接。
```

---

## 2. 适用题型

看到这些关键词，优先考虑链表：

```txt
删除节点
反转链表
合并链表
找链表中点
判断链表是否有环
倒数第 N 个节点
两个链表是否相交
```

常见技巧：

```txt
dummy 虚拟头节点：处理头节点变化、删除头节点、合并链表
prev / cur / next：反转链表、修改连接关系
快慢指针：找中点、判断环
前后指针：找倒数第 N 个节点
```

典型题目：

```txt
LeetCode 206：Reverse Linked List
LeetCode 21：Merge Two Sorted Lists
LeetCode 19：Remove Nth Node From End of List
LeetCode 876：Middle of the Linked List
LeetCode 141：Linked List Cycle
LeetCode 160：Intersection of Two Linked Lists
```

---

## 3. 经典例题和逐步拆解

### 经典例题：LeetCode 206. Reverse Linked List

题意：

给定一个单链表头节点 `head`，反转整个链表。

输入：

```txt
1 -> 2 -> 3 -> 4 -> 5 -> null
```

输出：

```txt
5 -> 4 -> 3 -> 2 -> 1 -> null
```

### 为什么需要 `prev / cur / next`？

如果当前节点是 `cur = 1`，链表是：

```txt
1 -> 2 -> 3 -> null
```

如果直接写：

```js
cur.next = prev;
```

那么 `1` 后面的 `2 -> 3` 可能就丢了。

所以要先保存：

```js
const next = cur.next;
```

再改指向。

### JavaScript 代码

```js
function reverseList(head) {
  let prev = null;
  let cur = head;

  while (cur !== null) {
    const next = cur.next; // 1. 保存后面的链表
    cur.next = prev;      // 2. 反转当前节点指向
    prev = cur;           // 3. prev 前进
    cur = next;           // 4. cur 前进
  }

  return prev;
}
```

### 逐步拆解

原链表：

```txt
1 -> 2 -> 3 -> null
```

初始：

```txt
prev = null
cur = 1
```

第 1 轮：

```txt
next = 2
1 -> null
prev = 1
cur = 2
```

此时：

```txt
已反转：1 -> null
未处理：2 -> 3 -> null
```

第 2 轮：

```txt
next = 3
2 -> 1 -> null
prev = 2
cur = 3
```

此时：

```txt
已反转：2 -> 1 -> null
未处理：3 -> null
```

第 3 轮：

```txt
next = null
3 -> 2 -> 1 -> null
prev = 3
cur = null
```

循环结束，返回 `prev`：

```txt
3 -> 2 -> 1 -> null
```

### 复杂度分析

```txt
时间复杂度：O(n)
空间复杂度：O(1)
```

每个节点只处理一次，只使用了有限几个指针变量。

---

## 4. 易错点

### 易错点 1：没保存 next，链表直接断掉

错误写法：

```js
cur.next = prev;
cur = cur.next;
```

这里的 `cur.next` 已经被改成 `prev`，原来的后续节点丢了。

正确顺序：

```js
const next = cur.next;
cur.next = prev;
prev = cur;
cur = next;
```

### 易错点 2：反转后返回 `head`

反转后，原来的 `head` 会变成尾节点。新的头节点是 `prev`：

```js
return prev;
```

不是：

```js
return head;
```

### 易错点 3：该改 next 时只移动了变量

删除节点时，错误写法：

```js
slow = slow.next.next;
```

这只是让变量 `slow` 跳走，没有改变链表结构。

真正删除节点：

```js
slow.next = slow.next.next;
```

### 易错点 4：滥用 dummy

`dummy` 很有用，但不是所有链表题都需要。

```txt
要删除节点、合并链表、头节点可能变化 -> 常用 dummy
只是找中点、判断环、不修改结构 -> 通常不用 dummy
```

### 易错点 5：判断节点相等时比较了 val

判断两个链表节点是否是同一个节点，要比较引用：

```js
slow === fast
```

不要比较：

```js
slow.val === fast.val
```

因为不同节点可以有相同值。

---

## 5. 今日练习题

### LeetCode 21：Merge Two Sorted Lists

题意：

给定两个升序链表 `list1` 和 `list2`，合并成一个新的升序链表。

示例：

```txt
list1 = 1 -> 2 -> 4
list2 = 1 -> 3 -> 4
```

输出：

```txt
1 -> 1 -> 2 -> 3 -> 4 -> 4
```

训练目标：

```txt
dummy 虚拟头节点
cur 指针拼接链表
移动 list1 / list2 指针
```

提示：

```txt
1. 创建 dummy
2. cur 指向 dummy
3. 比较 list1.val 和 list2.val
4. 谁小就接谁
5. 被接上的链表往后走一步
6. cur 也往后走一步
7. 最后把剩下的链表接上
```

代码：

```js
function mergeTwoLists(list1, list2) {
  const dummy = new ListNode(0);
  let cur = dummy;

  while (list1 !== null && list2 !== null) {
    if (list1.val <= list2.val) {
      cur.next = list1;
      list1 = list1.next;
    } else {
      cur.next = list2;
      list2 = list2.next;
    }

    cur = cur.next;
  }

  cur.next = list1 !== null ? list1 : list2;

  return dummy.next;
}
```

额外消化题：

```txt
LeetCode 876：Middle of the Linked List
LeetCode 141：Linked List Cycle
LeetCode 19：Remove Nth Node From End of List
```

---

## 6. 精华总结

链表题一句话：

```txt
链表没有下标，只能靠指针移动和修改 next 关系。
```

反转链表模板：

```js
let prev = null;
let cur = head;

while (cur !== null) {
  const next = cur.next;
  cur.next = prev;
  prev = cur;
  cur = next;
}

return prev;
```

合并链表模板：

```js
const dummy = new ListNode(0);
let cur = dummy;

// cur.next = 某个节点
// 被接上的链表向后走
// cur = cur.next

return dummy.next;
```

快慢指针找中点：

```js
let slow = head;
let fast = head;

while (fast !== null && fast.next !== null) {
  slow = slow.next;
  fast = fast.next.next;
}

return slow;
```

题型判断补充：

```txt
要改节点连接关系 -> 链表
要找中点 / 判断环 -> 快慢指针
要删除头节点也能统一处理 -> dummy
```
