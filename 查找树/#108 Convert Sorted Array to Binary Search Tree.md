[toc]

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.



## 题目解读

&emsp;给定增序数组，转化为高度平衡的二叉搜索树。

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
    public TreeNode sortedArrayToBST(int[] nums) {
        
    }
}
```

## 程序设计

* 题目限制高度平衡二叉树为两个子树之差不超过1，实际可以把排序数组看做完全二叉树，最中间的为根，依次类推。

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return sortedArrayToBST(nums, 0, nums.length - 1);
    }

    private TreeNode sortedArrayToBST(int[] nums, int start, int end) {
        if(start > end) {
            return null;
        }
        int mid = start + (end - start + 1) / 2;
        // 递归
        TreeNode head = new TreeNode(nums[mid]);
        head.left = sortedArrayToBST(nums, start, mid - 1);
        head.right = sortedArrayToBST(nums, mid + 1, end);
        return head;
    }
}
```

## 性能分析

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.6MB，在所有java提交中击败了5.05%的用户。

## 官方解题

&emsp;同上。