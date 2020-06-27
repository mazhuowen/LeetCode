[toc]

Return the root node of a binary **search** tree that matches the given `preorder` traversal.

(Recall that a binary search tree is a binary tree where for every `node`, any descendant of `node.left` has a value `< node.val`, and any descendant of `node.right` has a value `> node.val`.  Also recall that a preorder traversal displays the value of the `node` first, then traverses `node.left`, then traverses `node.right`.)

It's guaranteed that for the given test cases there is always possible to find a binary search tree with the given requirements.



**Constraints**:

* $1 \le \text{preorder.length} \le 100$
* $1 \le \text{preorder[i]} \le 10^8$
* The values of `preorder` are distinct.



## 题目解读

&emsp;根据前序序列构建二叉搜索树。

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
    public TreeNode bstFromPreorder(int[] preorder) {
        
    }
}
```

## 程序设计

* 由于是二叉查找树的前序序列，故数组第一个值是根节点，其后第一个值若小于根节点，则为左子树根节点，其后若存在大于根节点的值，则为右子树根节点。

```java
class Solution {
    public TreeNode bstFromPreorder(int[] preorder) {
        if (preorder == null || preorder.length == 0) return null;
        return bstFromPreorder(preorder, 0, preorder.length - 1);
    }

    private TreeNode bstFromPreorder(int[] preorder, int start, int end) {
        if (start > end) return null;

        TreeNode root = new TreeNode(preorder[start]);

        // 右子树根节点为数组中第一个大于当前节点的节点
        int right = start + 1;
        while (right <= end && preorder[right] < preorder[start]) right++;
        // 递归
        root.left = bstFromPreorder(preorder, start + 1, right - 1);
        root.right = bstFromPreorder(preorder, right, end);
        return root;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.7MB，在所有java提交中击败了11.11%的用户。

## 官方解题

&emsp;官方思路提出使用前序遍历和中序遍历来构造树，二叉搜索树的中序序列就是数组排序后的形式。除了这个基本思路，官方还提供了巧妙的递归实现：

```java
class Solution {
    int idx;
    int[] preorder;

    public TreeNode bstFromPreorder(int[] preorder) {
        if (preorder == null || preorder.length == 0) return null;

        this.preorder = preorder;
        return bstFromPreorder(Integer.MIN_VALUE, Integer.MAX_VALUE);
    }

    private TreeNode bstFromPreorder(int lower, int upper) {
        if (idx == preorder.length) return null;

        int val = preorder[idx];
        // 不在区间内，表示当前点不是该子树中的节点
        if (val < lower || val > upper) return null;

        // 在区间内，后移递归
        idx++;
        TreeNode root = new TreeNode(val);
        root.left = bstFromPreorder(lower, val);
        root.right = bstFromPreorder(val, upper);

        return root;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.9MB，在所有java提交中击败了11.11%的用户。

&emsp;迭代实现如下：

```java
class Solution {
    public TreeNode bstFromPreorder(int[] preorder) {
        if (preorder == null || preorder.length == 0) return null;

        Stack<TreeNode> stack = new Stack<>();
        TreeNode root = new TreeNode(preorder[0]);
        stack.push(root);

        for (int i = 1; i < preorder.length; i++) {
            TreeNode cur = new TreeNode(preorder[i]);

            // 前序遍历前一节点大于当前节点，则当前节点是左节点
            if (stack.peek().val > cur.val) {
                stack.peek().left = cur;
            }
            // 小于当前节点，说明当前节点是某个节点的右子节点
            // 根据二叉搜索树的前序序列，当前节点大于之前子树的所有节点，
            // 出栈直到遇到最后一个小于当前节点的节点，就是父节点
            else {
                TreeNode temp = null;
                while (!stack.isEmpty() && stack.peek().val < cur.val) temp = stack.pop();

                temp.right = cur;
            }

            stack.push(cur);
        }

        return root;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了17.57%的用户。

内存消耗：37.8MB，在所有java提交中击败了11.11%的用户。