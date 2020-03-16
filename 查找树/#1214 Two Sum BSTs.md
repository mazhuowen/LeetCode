[toc]

Given two binary search trees, return `True` if and only if there is a node in the first tree and a node in the second tree whose values sum up to a given integer `target`.

Constraints:

* Each tree has at most `5000` nodes.
* $-10^9 \le \text{target}, \text{node.val} \le 10^9$



## 题目解读

&emsp;从两个二叉查找树搜索是否存在和为指定值的组合。

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
    public boolean twoSumBSTs(TreeNode root1, TreeNode root2, int target) {

    }
}
```

## 程序设计

* 首先想到的是两个树同时遍历，但是时间复杂度为$O(NM)$，会超时。

```java
class Solution {
    public boolean twoSumBSTs(TreeNode root1, TreeNode root2, int target) {
        if(root1 == null || root2 == null) return false;
        if(root1.val + root2.val == target) return true;
        if(root1.val + root2.val > target) {
            return twoSumBSTs(root1, root2.left, target) || twoSumBSTs(root1.left, root2, target);
        } else {
            return twoSumBSTs(root1.right, root2, target) || twoSumBSTs(root1, root2.right, target);
        }
    }
}
```

* 简化思路，遍历一个树，同时查找另一个树。

```java
class Solution {
    public boolean twoSumBSTs(TreeNode root1, TreeNode root2, int target) {
        if(root1 == null || root2 == null) return false;
        if(find(root2, target - root1.val)) {
            return true;
        }
        return twoSumBSTs(root1.left, root2, target) || twoSumBSTs(root1.right, root2, target);
    }

    private boolean find(TreeNode root, int target) {
        if(root == null) return false;
        if(root.val == target) return true;
        if(root.val > target) {
            return find(root.left, target);
        } else {
            return find(root.right, target);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2M)$，空间复杂度为$O(max(\log_2N, \log_2M)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.1MB，在所有java提交中击败了6.67%的用户。

## 官方解题

&emsp;暂无，密切关注。