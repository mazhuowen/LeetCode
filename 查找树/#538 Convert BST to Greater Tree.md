[toc]

Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.



## 题目解读

&emsp;将二叉查找树转化为累加树，即当前结点加上比其大的结点值之和。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode convertBST(TreeNode root) {

    }
}
```

## 程序设计

* 实质就是反向中序遍历时每个结点加上前面序列的和。

```java
class Solution {
    private long preSum = 0;

    public TreeNode convertBST(TreeNode root) {
        if(root == null) {
            return root;
        }
        // 先遍历大的结点
        convertBST(root.right);
        // 更新结点值和和
        int temp = root.val;
        root.val += preSum;
        preSum += temp;
        // 后遍历小的结点
        convertBST(root.left);
        return root;
    }
}
```

* 可以使用莫里斯中序遍历实现：

```java
class Solution {
    public TreeNode convertBST(TreeNode root) {
        if(root == null) {
            return null;
        }
        long preSum = 0;
        TreeNode temp = root;
        while(temp != null) {
            if(temp.right != null) {
                TreeNode cur = temp.right;
                while(cur.left != null && cur.left != temp) {
                    cur = cur.left;
                }
                if(cur.left == null) {
                    cur.left = temp;
                    temp = temp.right;
                } else {
                    int val = temp.val;
                    temp.val += preSum;
                    preSum += val;
                    cur.left = null;
                    temp = temp.left;
                }
            } else {
                int val = temp.val;
                temp.val += preSum;
                preSum += val;
                temp = temp.left;
            }
        }
        return root;
    }
}
```

## 性能分析

&emsp;递归时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了99.68%的用户。

内存消耗：41.1MB，在所有java提交中击败了33.66%的用户。

&emsp;迭代方法时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.68%的用户。

内存消耗：41.5MB，在所有java提交中击败了28.23%的用户。

## 官方解题

&emsp;同上。