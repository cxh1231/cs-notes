## 1、题目描述

二叉树定义：

```java
class TreeNode {

    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
```

+ 时间限制：1秒

+ 空间限制：256M

## 2、解题思路

解题思路

## 3、代码实现

```java
public class Solution {

    int ans;
    int current;

    public int KthNode(TreeNode proot, int k) {
        // 初始化答案和当前值（倒着减）
        ans = -1;
        current = k;
        // 递归，后序遍历
        order(proot);
        return ans;
    }

    private void order(TreeNode root) {
        // 如果节点为空，则直接返回
        if (root == null) {
            return;
        }
        // 先遍历右节点（右、中、左，倒序遍历）
        order(root.left);
        // 遍历次数减一
        --current;
        if (current == 0) {
            ans = root.val;
            return;
        }
        // 左
        order(root.right);
    }
}
```

## 4、复杂度分析

+ 时间复杂度：

+ 空间复杂度：