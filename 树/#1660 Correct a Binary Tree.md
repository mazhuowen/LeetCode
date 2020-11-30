[toc]

You have a binary tree with a small defect. There is **exactly one** invalid node where its right child incorrectly points to another node at the **same depth** but to the **invalid node's right**.

Given the root of the binary tree with this defect, `root`, return the root of the binary tree after **removing** this invalid node **and every node underneath it** (minus the node it incorrectly points to).

**Custom testing**:

The test input is read as $3$ lines:

* `TreeNode root`
* `int fromNode` (**not available to** `correctBinaryTree`)
* `int toNode` (**not available to** `correctBinaryTree`)

After the binary tree rooted at `root` is parsed, the `TreeNode` with value of `fromNode` will have its right child pointer pointing to the `TreeNode` with a value of `toNode`. Then, `root` is passed to `correctBinaryTree`.

 

**Example 1**:

<img src="..\images\#1660_exp1.png" style="zoom: 80%;" />

```
Input: root = [1,2,3], fromNode = 2, toNode = 3
Output: [1,null,3]
Explanation: The node with value 2 is invalid, so remove it.
```

**Example 2**:

<img src="..\images\#1660_exp2.png" style="zoom: 50%;" />

```
Input: root = [8,3,1,7,null,9,4,2,null,null,null,5,6], fromNode = 7, toNode = 4
Output: [8,3,1,null,null,9,4,null,null,5,6]
Explanation: The node with value 7 is invalid, so remove it and the node underneath it, node 2.
```



**Constraints**:

* The number of nodes in the tree is in the range $[3, 10^4]$.
* $-10^9 \le \text{Node.val} \le 10^9$
* All `Node.val` are **unique**.
* $\text{fromNode} \ne \text{toNode}$
* `fromNode` and `toNode` will exist in the tree and will be on the same depth.
* `toNode` is to the **right** of `fromNode`.
* `fromNode.right` is `null` in the initial tree from the test data.



## 题目解读

&emsp;给定二叉树，有一个节点错误的指向了同层右侧的节点，删除该节点和子树，返回根节点。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode correctBinaryTree(TreeNode root) {
        
    }
}
```

## 程序设计

* 层次遍历，使用哈希表记录同层遍历过的节点，如果当前节点的子节点包含同层的值，就是错误节点。

```java
class Solution {
    public TreeNode correctBinaryTree(TreeNode root) {
        if (root == null) return root;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        // 记录父节点，方便删除子树
        Map<Integer, TreeNode> parents = new HashMap<>();
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode cur = queue.poll();
                if (cur.right != null) {
                    // 错误节点，指向了同层节点
                    if (parents.containsKey(cur.right.val)) {
                        cur.right = null;
                        // 获取父节点，删除子树
                        TreeNode parent = parents.get(cur.val);
                        if (parent.left == cur) parent.left = null;
                        else parent.right = null;
                        return root;
                    }
                    parents.put(cur.right.val, cur);
                    queue.add(cur.right);
                }
                if (cur.left != null) {
                    parents.put(cur.left.val, cur);
                    queue.add(cur.left);
                }
            }
        }
        return root;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：9 ms, 在所有 Java 提交中击败了90.91%的用户

内存消耗：41.3 MB, 在所有 Java 提交中击败了90.00%的用户

## 官方解题

&emsp;暂无，密切关注。社区思路类似。