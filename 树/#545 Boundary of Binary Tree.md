[toc]

Given a binary tree, return the values of its boundary in **anti-clockwise** direction starting from root. Boundary includes left boundary, leaves, and right boundary in order without duplicate **nodes**.  (The values of the nodes may still be duplicates.)

**Left boundary** is defined as the path from root to the **left-most** node. **Right boundary** is defined as the path from root to the **right-most** node. If the root doesn't have left subtree or right subtree, then the root itself is left boundary or right boundary. Note this definition only applies to the input binary tree, and not applies to any subtrees.

The **left-most** node is defined as a **leaf** node you could reach when you always firstly travel to the left subtree if exists. If not, travel to the right subtree. Repeat until you reach a leaf node.

The **right-most** node is also defined by the same way with left and right exchanged.



## 题目解读

&emsp;给定树，根据逆时针反向遍历边界，即先左支路自顶向下，后叶节点自左至右，最后右支路自底向上。

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
    public List<Integer> boundaryOfBinaryTree(TreeNode root) {

    }
}
```

## 程序设计

* 注意到除了右支路先遍历子节点再遍历父结点，左支路先遍历父结点再遍历子节点，其它结点都是先左子节点后右子节点，可以使用递归实现，但要标识左边还是右边还是内部结点。

```java
class Solution {
    public List<Integer> boundaryOfBinaryTree(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        res.add(root.val);
        boundaryOfBinaryTree(root.left, 1, res);
        boundaryOfBinaryTree(root.right, -1, res);
        return res;
    }
    // 使用flag标识来区分左路径、右路径和内部结点
    private void boundaryOfBinaryTree(TreeNode root, int flag, List<Integer> res) {
        // 递归终止条件
        if(root == null) {
            return;
        }
        // 左支路
        if(flag == 1) {
            res.add(root.val);
            // 左支路先遍历父节点再遍历子节点
            // 注意子节点左支路的判断，没有左子节点则右子节点是左支路，否则是内部结点
            if(root.left != null) {
                boundaryOfBinaryTree(root.left, 1, res);
                boundaryOfBinaryTree(root.right, 0, res);
            } else {
                boundaryOfBinaryTree(root.right, 1, res);
            }
        } 
        // 右支路先遍历子节点再遍历父节点，判断同理
        else if(flag == -1) {
            if(root.right != null) {
                boundaryOfBinaryTree(root.left, 0, res);
                boundaryOfBinaryTree(root.right, -1, res);
            } else{
                boundaryOfBinaryTree(root.left, -1, res);
            }
            res.add(root.val);
        } 
        // 内部结点只有叶节点才会加入
        else {
            if(root.left == null && root.right == null) {
                res.add(root.val);
            }
            boundaryOfBinaryTree(root.left, 0, res);
            boundaryOfBinaryTree(root.right, 0, res);
        }
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.1MB，在所有java提交中击败了7.14%的用户。

## 官方解题

&emsp;除了上述思路，还提供了最基础的，先遍历左支路，再遍历叶结点，最后遍历右支路的方法，分成三步进行。