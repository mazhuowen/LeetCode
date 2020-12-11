[toc]

Given the `root` of a binary tree, return the lowest common ancestor (LCA) of two given nodes, `p` and `q`. If either node `p` or `q` **does not exist** in the tree, return `null`. All values of the nodes in the tree are **unique**.

According to the definition of LCA on Wikipedia: "The lowest common ancestor of two nodes `p` and `q` in a binary tree `T` is the lowest node that has both `p` and `q` as **descendants** (where we allow **a node to be a descendant of itself**)". A **descendant** of a node `x` is a node `y` that is on the path from node `x` to some leaf node.

 

**Example 1**:

<img src="..\images\#1644_exp1.png"  />

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2**:

<img src="..\images\#1644_exp2.png"  />

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5. A node can be a descendant of itself according to the definition of LCA.
```

**Example 3**:

<img src="..\images\#1644_exp3.png"  />

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 10
Output: null
Explanation: Node 10 does not exist in the tree, so return null.
```



**Constraints**:

* The number of nodes in the tree is in the range $[1, 10^4]$.
* $-10^9 \le \text{Node.val} \le 10^9$
* All `Node.val` are **unique**.
* $p \ne q$



**Follow up**: Can you find the LCA traversing the tree, without checking nodes existence?



## 题目解读

&emsp;寻找两个节点的最近公共父节点。

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

* 采用前序遍历，第一个同时拥有两个节点的节点就是最近公共父节点。

```java
class Solution {
    private TreeNode lca = null;

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        lca(root, p, q);
        return lca;
    }

    // 返回1表示子树包含p，2表示包含q，0不包含，3包含两个节点
    private int lca(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return 0;
        int res = root == p ? 1 : (root == q ? 2 : 0);
        res += lca(root.left, p, q);
        if (res != 3) res += lca(root.right, p, q);
        // 第一次为3，表明是最近公共父节点
        if (res == 3 && lca == null) lca = root;
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：8 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：41.5 MB, 在所有 Java 提交中击败了40.00%的用户。

## 官方解题

&emsp;暂无，密切关注。
