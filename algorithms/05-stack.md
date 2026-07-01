# 05. 栈 Stack

## 1. 核心思想

栈的核心是：

```txt
后进先出，Last In First Out
```

它最适合处理：

```txt
最近出现、但还没被匹配或解决的元素
```

JavaScript 常用数组模拟栈：

```js
const stack = []

stack.push(x)
stack.pop()
stack[stack.length - 1]
```

## 2. 适用题型

看到这些信号，优先想栈：

```txt
括号匹配
最近未匹配
相邻抵消
撤销操作
路径简化
表达式计算
单调栈
下一个更大元素
每日温度
```

典型 LeetCode：

```txt
20. Valid Parentheses
71. Simplify Path
150. Evaluate Reverse Polish Notation
155. Min Stack
496. Next Greater Element I
739. Daily Temperatures
1047. Remove All Adjacent Duplicates In String
```

## 3. 经典例题和逐步拆解

### 例题：LeetCode 20. Valid Parentheses

题意：

```txt
判断括号字符串是否合法。
左括号必须由同类型右括号闭合，且顺序正确。
```

示例：

```txt
s = "{[]}"
输出：true
```

```txt
s = "([)]"
输出：false
```

### 思路

遇到左括号：

```txt
入栈
```

遇到右括号：

```txt
弹出最近的左括号，检查是否匹配
```

### 模板代码

```js
var isValid = function(s) {
    const stack = []
    const pair = {
        "(": ")",
        "[": "]",
        "{": "}"
    }

    for (const char of s) {
        if (char === "(" || char === "[" || char === "{") {
            stack.push(char)
        } else {
            if (stack.length === 0) return false

            const top = stack.pop()

            if (pair[top] !== char) {
                return false
            }
        }
    }

    return stack.length === 0
};
```

### 逐步拆解

输入：

```txt
s = "{[]}"
```

过程：

```txt
遇到 {：入栈 ["{"]
遇到 [：入栈 ["{", "["]
遇到 ]：弹出 "["，匹配成功
遇到 }：弹出 "{"，匹配成功
最后栈为空
```

返回：

```txt
true
```

错误例子：

```txt
s = "([)]"
```

过程：

```txt
遇到 (：入栈
遇到 [：入栈
遇到 )：弹出 "["，但 "[" 不能匹配 ")"
```

返回：

```txt
false
```

### 复杂度

```txt
时间复杂度：O(n)
空间复杂度：O(n)
```

## 4. 易错点

### 易错点 1：右括号来时，栈可能为空

比如：

```txt
s = "]"
```

需要直接返回：

```js
if (stack.length === 0) return false
```

### 易错点 2：最后必须检查栈是否为空

比如：

```txt
s = "((("
```

过程中没有类型冲突，但最后没有闭合。

所以：

```js
return stack.length === 0
```

### 易错点 3：括号数量对，不代表合法

比如：

```txt
s = "([)]"
```

数量没问题，但顺序错了。

## 5. 今日练习题

### LeetCode 1047. Remove All Adjacent Duplicates In String

不断删除相邻且相同的两个字符。

提示：

```txt
遍历字符：
如果栈顶和当前字符一样，pop
否则 push
```

参考模板：

```js
var removeDuplicates = function(s) {
    const stack = []

    for (const char of s) {
        const top = stack[stack.length - 1]

        if (top === char) {
            stack.pop()
        } else {
            stack.push(char)
        }
    }

    return stack.join("")
};
```

## 6. 精华总结

栈题识别口诀：

```txt
最近未匹配，用栈。
```

常见动作：

```txt
遇到暂时不能解决的元素，先入栈
遇到能解决它的元素，处理栈顶
```

一句话：

```txt
栈解决的是“最后来的，最先处理”。
```
