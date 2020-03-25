[toc]

Given the `root` of a binary tree, each node in the tree has a distinct value.

After deleting all nodes with a value in `to_delete`, we are left with a forest (a disjoint union of trees).

Return the roots of the trees in the remaining forest.  You may return the result in any order.



Constraints:

* The number of nodes in the given tree is at most `1000`.
* Each node has a distinct value between `1` and `1000`.
* $\text{to_delete.length} \le 1000$
* `to_delete` contains distinct values between `1` and `1000`.



## 题目解读

&emsp;给定二叉树和要删除的结点，返回删除后形成的森林。

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
    public List<TreeNode> delNodes(TreeNode root, int[] to_delete) {

    }
}
```

## 程序设计

* 采用后序遍历的形式检查删除点，然后将是否删除的结果告知父节点，交由父节点处理。特别需注意，最后要单独处理根节点的情况。

```java
class Solution {
    public List<TreeNode> delNodes(TreeNode root, int[] to_delete) {
        List<TreeNode> res = new LinkedList<>();
        if (root == null) return res;
        Set<Integer> delete = new HashSet<>();
        for (int val : to_delete) delete.add(val);

        // 如果根节点不删除，需要将根节点加入
        boolean delRoot = delNode(root, delete, res);
        if (!delRoot) res.add(root);
        return res;
    }

    // 返回是否删除当前结点
    private boolean delNode(TreeNode root, Set<Integer> delete, List<TreeNode> res) {
        if (root == null) return true;
        boolean delLeft = delNode(root.left, delete, res);
        boolean delRight = delNode(root.right, delete, res);
        // 删除需要删除的子节点
        if (delLeft) root.left = null;
        if (delRight) root.right = null;

        // 当前结点需要删除，维护新的树，加入链表
        if (delete.contains(root.val)) {
            if (!delLeft) res.add(root.left);
            if (!delRight) res.add(root.right);
            return true;
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：2ms，在所有java提交中击败了97.45%的用户。

内存消耗：41.7MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;暂无，密切关注。