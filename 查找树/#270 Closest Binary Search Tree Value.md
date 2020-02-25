[toc]

Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

Note:

* Given target value is a floating point.
* You are guaranteed to have only one unique value in the BST that is closest to the target.



## 题目解读

&emsp;找到与目标值最接近的值。

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
    public int closestValue(TreeNode root, double target) {

    }
}
```

## 程序设计

* 搜索问题的变形，记录搜索路径上数字与目标数字的差值并更新。

```java
class Solution {
    public int closestValue(TreeNode root, double target) {
        double result = Double.MAX_VALUE;
        while(root != null) {
            // 相等，直接返回
            if(target == root.val) {
                result = root.val;
                break;
            }
            // 更新最接近的值
            if(Math.abs(root.val - target) < Math.abs(result - target)) {
                result = root.val;
            }
            // 迭代搜索
            if(target < root.val) {
                root = root.left;
            } else {
                root = root.right;
            }
        }
        return (int)result;
    }
}
```

> 特别注意整数溢出的问题，result采用浮点数

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，最坏$O(N)$；空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39MB，在所有java提交中击败了5.44%的用户。

## 官方解题

&emsp;同上。