[toc]

In a binary tree, the root node is at depth 0, and children of each depth `k` node are at depth `k+1`.

Two nodes of a binary tree are cousins if they have the same depth, but have **different parents**.

We are given the `root` of a binary tree with unique values, and the values `x` and `y` of two different nodes in the tree.

Return `true` if and only if the nodes corresponding to the values `x` and `y` are cousins.



Note:

* The number of nodes in the tree will be between `2` and `100`.
* Each node has a unique integer value from `1` to `100`.



## 题目解读

&emsp;判断是否是表亲结点，表亲结点定义为同层非同父的结点。

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
    public boolean isCousins(TreeNode root, int x, int y) {

    }
}
```

## 程序设计

* 层次遍历。

```java
class Solution {
    public boolean isCousins(TreeNode root, int x, int y) {
        if (root == null) return false;
        Queue<Pair> queue = new LinkedList<>();
        queue.add(new Pair(root, -1));
        // 标识，表示有元素在当前层，另一个元素如果是表亲必须在该层
        boolean flag = false;
        // 记录在当前层元素的父结点
        int parent = -1;
        while (!queue.isEmpty() && !flag) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                Pair cur = queue.poll();
                if (cur.key.val == x || cur.key.val  == y) {
                    // 匹配到第一个元素，记录
                    if (flag) return parent != cur.val;
                    // 匹配到第二个元素，更新
                    else {
                        flag = true;
                        parent = cur.val;
                    }
                }
                // 入队
                if (cur.key.left != null) queue.add(new Pair(cur.key.left, cur.key.val));
                if (cur.key.right != null) queue.add(new Pair(cur.key.right, cur.key.val));  
            }
        }
        return false;
    }
}

class Pair {
    TreeNode key;
    int val;

    Pair(TreeNode key, int val) {
        this.key = key;
        this.val = val;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了66.33%的用户。

内存消耗：37.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方思路通过哈希表记录每个结点的深度和父节点。