[toc]

Given the `root` of a binary tree and a `leaf` node, reroot the tree so that the `leaf` is the new root.

You can reroot the tree with the following steps for each node `cur` on the path **starting from the** `leaf` up to the `root` **excluding the root**:

1. If `cur` has a left child, then that child becomes `cur`'s right child. Note that it is guaranteed that `cur` will have at most one child.
2. `cur`'s original parent becomes `cur`'s left child.

Return the new root of the rerooted tree.



**Note**: Ensure that your solution sets the `Node.parent` pointers correctly after rerooting or you will receive "Wrong Answer".

 

**Example 1**:

<img src="..\images\#1666_exp1.png" style="zoom: 80%;" />

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], leaf = 7
Output: [7,2,null,5,4,3,6,null,null,null,1,null,null,0,8]
```

**Example 2**:

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], leaf = 0
Output: [0,1,null,3,8,5,null,null,null,6,2,null,null,7,4]
```



**Constraints**:

* The number of nodes in the tree is in the range $[2, 100]$.
* $-10^9 \le \text{Node.val} \le 10^9$
* All `Node.val` are **unique**.
* `leaf` exist in the tree.



## 题目解读



```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
};
*/

class Solution {
    public Node flipBinaryTree(Node root, Node leaf) {
        
    }
}
```

## 程序设计

* 最基本的思路是先从叶节点转换到根节点，维护好新的左右子节点关系；然后再次遍历维护好父节点指针；由于迭代需要从底到顶，不能在迭代过程中修改父节点关系，故需要两步完成；
* 需注意转换条件中把根节点排除在外。

```java
class Solution {
    public Node flipBinaryTree(Node root, Node leaf) {
        if (root == null) throw new IllegalArgumentException("invalid param");

        Node cur = leaf;
        while (cur != null) {
            Node parent = cur.parent;
            // 断开旧连接，避免循环遍历
            if (parent != null) {
                if (parent.left == cur) parent.left = null;
                else parent.right = null;
            }
            // 新连接（转换关系排除根节点）
            if (cur != root) {
                if (cur.left != null) cur.right = cur.left;
                cur.left = parent;
            }
            // 迭代
            cur = parent;
        }
        // 维护父节点
        reverseParent(leaf);
        // 根节点的父节点为空
        leaf.parent = null;
        return leaf;
    }

    private void reverseParent(Node root) {
        if (root == null) return;
        Node originParent = root.parent;
        reverseParent(originParent);
        if (originParent != null) originParent.parent = root;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：16 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：37.5 MB, 在所有 Java 提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区有采用递归的方式完成解题。

```java
class Solution {
    public Node flipBinaryTree(Node root, Node leaf) {
        if (root == null || root == leaf) return root;

        // 维护当前叶子节点与父节点的关系
        Node parent = leaf.parent;
        leaf.parent = null;
        // 断开旧连接
        if (parent.left == leaf) parent.left = null;
        else parent.right = null;
        // 左子节点变为右子节点
        if (leaf.left != null) leaf.right = leaf.left;
        // 原先的父节点变为左子节点
        Node newChild = flipBinaryTree(root, parent);
        leaf.left = newChild;
        newChild.parent = leaf;
        return leaf;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：16 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：37.5 MB, 在所有 Java 提交中击败了100.00%的用户。