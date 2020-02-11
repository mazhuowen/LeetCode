[toc]

Given a binary tree where all the right nodes are either leaf nodes with a sibling (a left node that shares the same parent node) or empty, flip it upside down and turn it into a tree where the original right nodes turned into left leaf nodes. Return the new root.



## 题目解读

&emsp;题目给定一个二叉树，其中所有的右节点要么是具有兄弟节点（拥有相同父节点的左节点）的叶节点，要么为空，实质上就是左结点必须要有两个子节点，右结点要么两个子节点要么本身是叶结点，不存在一个子节点的情况。将此二叉树上下翻转并将它变成一棵树， 原来的右节点将转换成左叶节点。返回新的根。

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
    public TreeNode upsideDownBinaryTree(TreeNode root) {
        
    }
}
```

## 程序设计

* 根据题目要求的树结构，考虑最简单的情况，一个根节点有两个子节点，则转换过程为：左子节点为新的根，右子节点为新的根的左子节点，原先的根则变为右子节点。对于一颗二叉树，可以从底到顶依次转换。
* 从低到顶可以用递归实现。

```java
class Solution {
    public TreeNode upsideDownBinaryTree(TreeNode root) {
        if(root == null) {
            return null;
        }
        // 叶节点
        if(root.left == null && root.right == null) {
            return root;
        }
        // 根据题目要求，必然存在两个子节点
        // 返回的最左边的结点就是新的根
        TreeNode left = upsideDownBinaryTree(root.left);
        // 注意对右子树不做任何处理
         // 转换
         root.left.left = root.right;
         root.left.right = root;
         // 原先的根节点断开连接，避免造成死循环（实质是斩断根节点的链接，其它结点在转换时自动斩断）
         root.left = null;
         root.right = null;
         // 返回根
        return left;
    }
}
```

> 根据题目样例，比如`[1,2,3,4,5,null,null,6,7,8,9]`，事实上对右子树$8 \gets 5 \to 9$不做任何转换，即题目只需对二叉树左支路上的结点及其两个子节点做转换，最后结果为`[6,7,4,null,null,5,2,null,null,null,null,8,9,3,1]`。

* 有了这个发现可以将上述递归优化为如下：

```java
class Solution {
    public TreeNode upsideDownBinaryTree(TreeNode root) {
        return upsideDownBinaryTree(null, root, null);
    }

    private TreeNode upsideDownBinaryTree(TreeNode parent, TreeNode left, TreeNode right) {
        // 如果左子节点为空，表明parent是叶节点或者空
        if(left == null) {
           return parent;
        }
        // 继续向左遍历，不管右子树
        TreeNode head = upsideDownBinaryTree(left, left.left, left.right);
        // 递归转换完底下的结点再转换当前结点
        left.left = right;
        left.right = parent;
        return head;
    }
}
```

* 有了上述思路可以使用迭代方法实现：

```java
class Solution {
    public TreeNode upsideDownBinaryTree(TreeNode root) {
        if(root == null || root.left == null) {
            return root;
        }
        TreeNode left = root.left, right = root.right;
        // 断掉头结点的链接，避免遍历新的树时引起死循环（其它结点的链接在转换过程中自然断掉）
        root.left = null;
        root.right = null;
        while(left != null) {
            // 记录后续转换的左右结点（因为本次转换会断掉子节点的连接）
            TreeNode leftTemp = left.left;
            TreeNode rightTemp = left.right;
            // 转换
            left.left = right;
            left.right = root;
            // 递归，root为新的根节点
            root = left;
            // 左右结点为后续左右结点
            left = leftTemp;
            right = rightTemp;
        }
        return root;
    }
}
```

## 性能分析

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时 :0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗 :40.5 MB, 在所有 Java 提交中击败了5.47%的用户。

&emsp;非递归方法时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.6MB，在所有java提交中击败了5.47%的用户。

## 官方解题

&emsp;暂无，密切关注。