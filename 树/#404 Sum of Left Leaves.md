[toc]

Find the sum of all left leaves in a given binary tree.



## 题目解读

&emsp;给定二叉树，计算所有左叶结点的和。

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
    public int sumOfLeftLeaves(TreeNode root) {
        
    }
}
```

## 程序设计

* 前序遍历计算和。递归实现：

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int sum = 0;
        // 判断左子节点是否是叶节点
        if(root.left != null && root.left.left == null && root.left.right == null) {
            sum += root.left.val;
        }
        sum += sumOfLeftLeaves(root.left);
        sum += sumOfLeftLeaves(root.right);
        return sum;
    }
}
```

* 非递归实现：

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int sum = 0;
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            System.out.println(cur.val);
            if(cur.left != null) {
                // 叶节点判断，并更新值
                if(cur.left.left == null & cur.left.right == null) {
                    sum += cur.left.val;
                } else {
                    stack.push(cur.left);
                }
            }
            if(cur.right != null) {
                stack.push(cur.right);
            }
        }
        return sum;
    }
}
```

## 性能分析

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.5MB，在所有java提交中击败了5.07%的用户。

&emsp;非递归方法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：14ms，在所有java提交中击败了14.72%的用户。

内存消耗：42.3MB，在所有java提交中击败了5.07%的用户。

## 官方解题

&emsp;暂无，密切关注。