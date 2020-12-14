[toc]

Given a binary tree with $N$ nodes, each node has a different value from $\{1, \dots, N\}$.

A node in this binary tree can be flipped by swapping the left child and the right child of that node.

Consider the sequence of $N$ values reported by a preorder traversal starting from the root.  Call such a sequence of $N$ values the voyage of the tree.

(Recall that a preorder traversal of a node means we report the current node's value, then preorder-traverse the left child, then preorder-traverse the right child.)

Our goal is to flip the **least number** of nodes in the tree so that the `voyage` of the tree matches the voyage we are given.

If we can do so, then return a list of the values of all nodes flipped.  You may return the answer in any order.

If we cannot do so, then return the list $[-1]$.

 

**Example 1**:

<img src="..\images\#971_exp1.png" style="zoom: 67%;" />

```
Input: root = [1,2], voyage = [2,1]
Output: [-1]
```

**Example 2**:

<img src="..\images\#971_exp2.png" style="zoom: 67%;" />

```
Input: root = [1,2,3], voyage = [1,3,2]
Output: [1]
```

**Example 3**:

<img src="..\images\#971_exp3.png" style="zoom: 67%;" />

```
Input: root = [1,2,3], voyage = [1,2,3]
Output: []
```



**Note**:

* $1 \le N \le 100$



## 题目解读

&emsp;可交换二叉树左右子节点，判断是否可变换二叉树得到指定的前序序列。

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
    public List<Integer> flipMatchVoyage(TreeNode root, int[] voyage) {

    }
}
```

## 程序设计

* 前序遍历，遇到不匹配的节点则尝试交换左右子节点（交换遍历顺序）进行匹配；
* 注意上述思路的前提是每个节点的值都不同。

```java
class Solution {
    int[] voyage;
    int idx = 0;

    public List<Integer> flipMatchVoyage(TreeNode root, int[] voyage) {
        List<Integer> res = new LinkedList<>();
        this.voyage = voyage;
        // 可交换得到
        if (preOrder(root, res)) return res;
        // 不可交换得到
        res.add(-1);
        return res;
    }

    private boolean preOrder(TreeNode root, List<Integer> res) {
        if (root == null) return true;
        if (root.val != voyage[idx++]) {
            res.clear();
            return false;
        }
        // 需要交换左右节点
        if (root.left != null && root.left.val != voyage[idx]) {
            // 添加并判断
            res.add(root.val);
            if (!preOrder(root.right, res) || !preOrder(root.left, res)) return false;
            else return true;
        }
        return preOrder(root.left, res) && preOrder(root.right, res);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：38.6 MB, 在所有 Java 提交中击败了69.46%的用户。

## 官方解题

&emsp;官方思路类似。
