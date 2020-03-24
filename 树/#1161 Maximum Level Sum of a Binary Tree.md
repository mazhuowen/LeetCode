[toc]

Given the `root` of a binary tree, the level of its root is `1`, the level of its children is `2`, and so on.

Return the **smallest** level `X` such that the sum of all the values of nodes at level `X` is **maximal**.



Note:

* The number of nodes in the given tree is between $1$ and $10^4$.
* $-10^5 \le \text{node.val} \le 10^5$



## 题目解读

&emsp;统计二叉树每层和最大的那层，如果存在多个，返回层数小的。

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
    public int maxLevelSum(TreeNode root) {

    }
}
```

## 程序设计

* 采用树的层次遍历，计算统计。

```java
class Solution {
    public int maxLevelSum(TreeNode root) {
        if (root == null) return -1;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int maxSum = Integer.MIN_VALUE, maxFloor = 0;
        int floor = 0;
        while (!queue.isEmpty()) {
            floor++;
            int sum = 0;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode cur = queue.poll();
                sum += cur.val;
                if (cur.left != null) queue.add(cur.left);
                if (cur.right != null) queue.add(cur.right);
            }
            if (sum > maxSum) {
                maxSum = sum;
                maxFloor = floor;
            }
        }
        return maxFloor;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：9ms，在所有java提交中击败了86.02%的用户。

内存消耗：43.6MB，在所有java提交中击败了6.15%的用户。

## 官方解题

&emsp;同上，还提供了前序遍历、中序遍历等思路，需要格外空间记录每一层的和，并动态操作。