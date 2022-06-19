## 1、题目描述

输入一个长度为 n 的链表，设链表中的元素的值为 $a_i$ ，返回该链表中倒数第k个节点。
如果该链表长度小于k，请返回一个长度为 0 的链表。

数据范围：

+ $ 0 ≤ n ≤ 10^5 $
+ $ 0 ≤ a_i ≤ 10^9 $
+ $ 0 ≤ k ≤ 10^9 $

例如输入 `head = [1,2,3,4,5], k = 2` 时，对应的链表结构如下图所示：

![](https://img.zxdmy.com/2022/202206191533053.png)

其中蓝色部分为该链表的最后2个结点，所以返回倒数第2个结点（也即结点值为4的结点）即可，系统会打印后面所有的节点来比较。

**示例1：**

```text
输入：head = [1,2,3,4,5], k = 2
输出：[4,5]
解释：返回倒数第2个节点4，系统会打印后面所有的节点来比较。
```

**示例2：**

```text
输入：head = [2], k = 8
输出：[]
```

**链表定义：**

```java
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}
```

要求：空间复杂度 O(n)，时间复杂度 O(n)

进阶：空间复杂度 O(1)，时间复杂度 O(n)

## 2、解题思路

解题思路

## 3、代码实现

```java
public class Solution {
    
    /**
     * @param pHead ListNode类 
     * @param k int整型 
     * @return ListNode类
     */
    public ListNode FindKthToTail(ListNode pHead, int k) {
        // 如果为空，则直接返回空
        if (pHead == null)
            return null;
        // 设置头，先让头指针走K步
        ListNode P1 = pHead;
        while (P1 != null && k-- > 0)
            P1 = P1.next;
        // 如果 走到结尾了，还没走完，说明K大于节点个数，返回空
        if (k > 0)
            return null;
        // 第二个节点开始行动
        ListNode P2 = pHead;
        // 直到第一个节点走到最后，才结束
        while (P1 != null) {
            P1 = P1.next;
            P2 = P2.next;
        }
        return P2;
    }
}
```

## 4、复杂度分析