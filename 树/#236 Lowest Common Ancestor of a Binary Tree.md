[toc]

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the `definition of LCA on Wikipedia`: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow **a node to be a descendant of itself**).”



Note:

* All of the nodes' values will be unique.
* p and q are different and both values will exist in the binary tree.



## 题目解读

&emsp;查找两个结点的共同祖先。

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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        
    }
}
```

## 程序设计

* 查找共同的祖先就是自底到顶遍历父结点的过程，可以使用递归实现。递归方法返回该结点路径中是否包含`p`或`q`，当前递归判断左右链路是否同时包含这两个点，包含则是公共最小祖先。

```java
class Solution {
    private TreeNode record;

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        traverseTree(root, p, q);
        return record;
    }

    // 当前结点子孙结点包含p或q则返回1，否则返回0
    private int traverseTree(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null) {
            return 0;
        }
        int left = traverseTree(root.left, p, q);
        int right = traverseTree(root.right, p, q);
        // 判断当前结点是否是其中一个点
        int cur = root == p || root == q ? 1 : 0;
        // 两个结点都在当前结点及其子树下
        if(left + right + cur == 2) {
            record = root;
        }
        // 返回1或0
        return (left + right + cur) > 0 ? 1 : 0;
    }
}
```

## 性能分析

&emsp;递归实现时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：9ms，在所有java提交中击败了75.80%的用户。

内存消耗：46.6MB，在所有java提交中击败了5.03%的用户。

## 官方解题

&emsp;除了上述递归思路，官方还提供了两种非递归实现。第一种方法引入字典保存结点父子关系，然后通过祖先结点的集合来判断LCA。

```java
class Solution {

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
		// 遍历寻找结点p、q
        Stack<TreeNode> stack = new Stack<>();
		// 保存结点的父节点关系
        Map<TreeNode, TreeNode> parent = new HashMap<>();
		// 初始化
        parent.put(root, null);
        stack.push(root);

        // 类似前序遍历，直到遍历找到两个结点为止
        while (!parent.containsKey(p) || !parent.containsKey(q)) {
            TreeNode node = stack.pop();
            if (node.left != null) {
                parent.put(node.left, node);
                stack.push(node.left);
            }
            if (node.right != null) {
                parent.put(node.right, node);
                stack.push(node.right);
            }
        }

        // 不相交集记录p的所有祖先结点
        Set<TreeNode> ancestors = new HashSet<>();
		// 将祖先结点加入不相交集
        while (p != null) {
            ancestors.add(p);
            p = parent.get(p);
        }
		// 遍历q的祖先结点，直到第一个与p祖先相交的点就是LCA
        while (!ancestors.contains(q))
            q = parent.get(q);
        return q;
    }
}

```

时间复杂度为$O(N)$，空间复杂度为$O(N)$。