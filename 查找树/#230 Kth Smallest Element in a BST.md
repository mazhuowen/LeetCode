[toc]

Given a binary search tree, write a function `kthSmallest` to find the kth smallest element in it.

Note:
You may assume k is always valid, $1 \le k \le \text{BST's total elements}$.



Follow up:
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?



##  题目解读

&emsp;查找二叉查找树中的第$k$小的数。

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
    public int kthSmallest(TreeNode root, int k) {

    }
}
```

## 程序设计

* 中序序列的性质。

```java
class Solution {
    private int kthSmall = -1;
    private int count = 0;
    
    public int kthSmallest(TreeNode root, int k) {
        countKthNum(root, k);
        return kthSmall;
    }

    private void countKthNum(TreeNode root, int k) {
        // 递归终止条件
        if(root == null) {
            return;
        }
        countKthNum(root.left, k);
        // 中序遍历当前元素，判断是否是第k个
        if(++count == k) {
            kthSmall = root.val;
            return;
        }
        countKthNum(root.right, k);
    }
}
```

* 可使用中序遍历的的迭代形式来计数判断，此处不再阐述。

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.4MB，在所有java提交中击败了5.00%的用户。

## 官方解题

&emsp;同上，官方解题采用链表保存中序序列，然后再取第$k$个。