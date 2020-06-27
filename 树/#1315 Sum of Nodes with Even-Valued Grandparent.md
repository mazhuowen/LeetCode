[toc]

Given a binary tree, return the sum of values of nodes with even-valued grandparent.  (A grandparent of a node is the parent of its parent, if it exists.)

If there are no nodes with an even-valued grandparent, return 0.

 

**Constraints**:

* The number of nodes in the tree is between $1$ and $10^4$.
* The value of nodes is between $1$ and $100$.



## 题目解读

&emsp;求祖父节点为偶数的孙子节点和。

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
    public int sumEvenGrandparent(TreeNode root) {

    }
}
```

## 程序设计

* 最基本的思路是后序遍历返回子节点和孙子节点值，通过判断当前节点是否是偶数来更新和。

```java
class Solution {
    int sum;

    public int sumEvenGrandparent(TreeNode root) {
        sumParentGrandparent(root);
        return sum;
    }

    private int[] sumParentGrandparent(TreeNode root) {
        if (root == null) return new int[2];

        int[] left = sumParentGrandparent(root.left);
        int[] right = sumParentGrandparent(root.right);
        // 奇数则求和
        if (root.val % 2 == 0) sum += left[1] + right[1];

        return new int[]{root.val, left[0] + right[0]};
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：2ms，在所有java提交中击败了42.28%的用户。

内存消耗：40.4MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方还提供了前序遍历的思路。

```java
class Solution {
    private int sum = 0;

    public int sumEvenGrandparent(TreeNode root) {
        if (root == null) {
            return 0;
        }
        boolean even = (root.val % 2 == 0);
        sumEvenGrandparent(root.left, even);
        sumEvenGrandparent(root.right, even);
        return sum;
    }

    private void sumEvenGrandparent(TreeNode root, boolean isEven) {
        if (root == null) {
            return;
        }
        if (root.left != null) {
            if (isEven) {
                sum += root.left.val;
            }
            sumEvenGrandparent(root.left, root.val % 2 == 0);
        }
        if (root.right != null) {
            if (isEven) {
                sum += root.right.val;
            }
            sumEvenGrandparent(root.right, root.val % 2 == 0);
        }
    }
}
```

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.9MB，在所有java提交中击败了100.00%的用户。