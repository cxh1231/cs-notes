## 1、题目描述

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的 min 函数。

输入操作时保证 pop、top 和 min 函数操作时，栈中一定有元素。

此栈包含的方法有：

+ push(value):将value压入栈中
+ pop():弹出栈顶元素
+ top():获取栈顶元素
+ min():获取栈中最小元素

**示例:**

```text
输入: ["PSH-1","PSH2","MIN","TOP","POP","PSH1","TOP","MIN"]
输出: -1,2,1,-1
解析:
"PSH-1"表示将-1压入栈中，栈中元素为-1
"PSH2"表示将2压入栈中，栈中元素为2，-1
“MIN”表示获取此时栈中最小元素==>返回-1
"TOP"表示获取栈顶元素==>返回2
"POP"表示弹出栈顶元素，弹出2，栈中元素为-1
"PSH1"表示将1压入栈中，栈中元素为1，-1
"TOP"表示获取栈顶元素==>返回1
“MIN”表示获取此时栈中最小元素==>返回-1
```

+ 数据范围：
    + 操作数量满足 $ 0 \le n \le 300 $
    + 输入的元素满足 $ |val| \le 10000 $

+ 复杂度要求：
    + 栈的各个操作的时间复杂度是 O(1)
    + 空间复杂度是 O(n)

+ 时间限制：1秒

+ 空间限制：64M

## 2、解题思路

解题思路

## 3、代码实现

```java
import java.util.Stack;

public class Solution {

    // 栈1：用来存原始数据
    Stack<Integer> dataStack = new Stack<Integer>();
    // 栈2：用来存至当前数据时的最小值
    Stack<Integer> minStack = new Stack<Integer>();

    public void push(int node) {
        dataStack.push(node);
        // 这一步太精彩了
        // 这个操作，使得data栈和min栈的长度始终相等
        // 但是，栈顶元素始终是最小值
        // 比 如 data栈 [4,8,9,3,6,1,4]
        // 存入的min栈为：[4,4,4,3,3,1,1]
        minStack.push(minStack.isEmpty() ? node : Math.min(minStack.peek(), node));
    }

    public void pop() {
        dataStack.pop();
        // 同时弹出，保证两个栈的长度相同
        minStack.pop();
    }

    public int top() {
        return dataStack.peek();

    }

    public int min() {
        return minStack.peek();

    }
}
```

## 4、复杂度分析

+ 时间复杂度：

+ 空间复杂度：