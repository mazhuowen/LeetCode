[toc]

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.



## 题目解读

&emsp;给定二叉树，结点数值代表房子可盗窃的金额。题目要求不能盗窃相邻父子结点，否则房屋会报警。需求出最大的盗窃金额。

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
    public int rob(TreeNode root) {
        
    }
}
```

## 程序设计

* 根据题目要求，如果盗窃了当前结点，则不能盗窃子节点，只能从孙子结点开始继续盗窃过程。

```java
class Solution {
    public int rob(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int sum = root.val;
        // 当前结点加上孙子结点链路的最大金额
        if(root.left != null) {
            sum += rob(root.left.left) + rob(root.left.right);
        }
        if(root.right != null) {
            sum += rob(root.right.left) + rob(root.right.right);
        }
        // 比较当前结点和子节点的值，返回大的那个
        return Math.max(sum, rob(root.left) + rob(root.right));
    }
}
```

* 由于一次递归需要遍历6次，而这些计算都是重复的，可以借助外部结构记录每个结点的结果，简化计算。

```java
class Solution {
    public int rob(TreeNode root) {
        Map<TreeNode, Integer> record = new HashMap<>();
        return rob(root, record);
    }

    public int rob(TreeNode root, Map<TreeNode, Integer> record) {
        if(root == null) {
            return 0;
        }
        if(record.get(root) != null) {
            return record.get(root);
        }
        int sum = root.val;
        // 当前结点加上孙子结点链路的最大金额
        if(root.left != null) {
            sum += rob(root.left.left, record) + rob(root.left.right, record);
        }
        if(root.right != null) {
            sum += rob(root.right.left, record) + rob(root.right.right, record);
        }
        // 比较当前结点和子节点的值，返回大的那个
        int result = Math.max(sum, rob(root.left, record) + rob(root.right, record));
        record.put(root, result);
        return result; 
    }
}
```

* 可以采用动态规划的思路来继续优化。实质上每个结点都有两个状态

```java
class Solution {
    public int rob(TreeNode root) {
        int[] record = rrob(root);
        return Math.max(record[0], record[1]);
    }

    private int[] rrob(TreeNode root) {
        // 0记录不盗窃当前结点的最大金额，1记录到位当前结点的最大金额
        int[] record = new int[]{0, 0};
        if(root == null) {
            return record;
        }
        int[] left = rrob(root.left);
        int[] right = rrob(root.right);
        // 不盗窃当前结点，最大值为两个子节点方案中最大值之和
        record[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        // 盗窃当前结点，则最大值为子节点不盗窃的最大值和当前值之和
        record[1] = root.val + left[0] + right[0];
        return record; 
    }
}
```

## 性能分析

&emsp;原始递归每遍历一个结点便从该结点出发向下遍历所有结点，时间复杂度为$O(N^N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：868ms，在所有java提交中击败了11.70%的用户。

内存消耗：47.6MB，在所有java提交中击败了5.08%的用户。

&emsp;经过额外存储优化，性能得到提升。

执行用时：4ms，在所有java提交中击败了59.86%的用户。

内存消耗：46.6MB，在所有java提交中击败了5.08%的用户。

&emsp;动态规划的递归时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了99.81%的用户。

内存消耗：48.5MB，在所有java提交中击败了5.08%的用户。

## 官方解题

&emsp;暂无，密切关注。

