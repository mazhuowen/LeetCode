Given an integer array with no duplicates. A maximum tree building on this array is defined as follow:

* The root is the maximum number in the array.
* The left subtree is the maximum tree constructed from left part subarray divided by the maximum number.
* The right subtree is the maximum tree constructed from right part subarray divided by the maximum number.

Construct the maximum tree by the given array and output the root node of this tree.

**Note:**

* The size of the given array will be in the range `[1,1000]`.



## 题目解读

&emsp;给定一个不含重复元素的整数数组，以此数组构建的最大二叉树。最大二叉树根是数组中最大的元素，左右子树的根则是根左右两侧最大的元素，以此类推。

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
    public TreeNode constructMaximumBinaryTree(int[] nums) {

    }
}
```

## 程序设计

* 首先想到的是递归解法。

```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return constructMaximumBinaryTree(nums, 0, nums.length - 1);
    }

    private TreeNode constructMaximumBinaryTree(int[] nums, int start, int end) {
        // 递归终止条件
        if(start > end) {
            return null;
        }
        // 查找最大元素索引
        int max = start;
        for(int i = start + 1; i <= end; i++) {
            if(nums[i] > nums[max]) {
                max = i;
            }
        }
        // 递归
        TreeNode root = new TreeNode(nums[max]);
        root.left = constructMaximumBinaryTree(nums, start, max - 1);
        root.right = constructMaximumBinaryTree(nums, max + 1, end);
        return root;
    }
}
```

## 性能分析

&emsp;递归遍历$N$次，每次需要遍历寻找最大值需要$\log_2N$，最坏情况是$N$，总的时间复杂度为$O(N\log_2N)$，最坏$O(N^2)$；空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：2ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.4MB，在所有java提交中击败了5.15%的用户。

## 官方解题

&emsp;同上。