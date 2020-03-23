[toc]

Given two binary search trees `root1` and `root2`.

Return a list containing all the integers from both trees sorted in **ascending** order.



Constraints:

* Each tree has at most `5000` nodes.
* Each node's value is between `[-10^5, 10^5]`.



## 题目解读

&emsp;将两个二叉查找树的所有值有序输出。

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
    public List<Integer> getAllElements(TreeNode root1, TreeNode root2) {

    }
}
```

## 程序设计

* 中序遍历然后合并。

```java
class Solution {
    public List<Integer> getAllElements(TreeNode root1, TreeNode root2) {
        // 中序遍历
        List<Integer> l1 = new LinkedList<>();
        List<Integer> l2 = new LinkedList<>();
        inOrder(root1, l1);
        inOrder(root2, l2);
        // 合并
        List<Integer> res = new LinkedList<>();
        int idx1 = 0, idx2 = 0;
        while (idx1 < l1.size() && idx2 < l2.size()) {
            if (l1.get(idx1) <= l2.get(idx2)) {
                res.add(l1.get(idx1++));
            } else {
                res.add(l2.get(idx2++));
            }
        }
        while (idx1 < l1.size()) {
            res.add(l1.get(idx1++));
        }
        while (idx2 < l2.size()) {
            res.add(l2.get(idx2++));
        }
        return res;
    }

    private void inOrder(TreeNode root, List<Integer> list) {
        if (root == null) return;
        inOrder(root.left, list);
        list.add(root.val);
        inOrder(root.right, list);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N + M)$，空间复杂度为$O(\max(N,M))$。

执行用时：1109ms，在所有java提交中击败了5.06%的用户。

内存消耗：44.3MB，在所有java提交中击败了5.02%的用户。

## 官方解题

&emsp;同上。