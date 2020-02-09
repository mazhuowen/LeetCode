[toc]

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

**Note:**
Bonus points if you could solve it both recursively and iteratively.



## 题目解读

&emsp;使用递归、非递归判断一颗二叉树是否是对称的。

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
    public boolean isSymmetric(TreeNode root) {
        
    }
}
```

## 程序设计

* 遍历实现就是比较两棵树，也就是比较根结点的左右子树。

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null) {
            return true;
        }
        return isSymmetric(root.left, root.right);
    }

    private boolean isSymmetric(TreeNode tree1, TreeNode tree2) {
        if(tree1 == null || tree2 == null) {
            return tree1 == tree2;
        }
        if(tree1.val != tree2.val) {
            return false;
        }
        // 遍历比较，需注意树1和树2要比较相反的子树
        if(!isSymmetric(tree1.left, tree2.right)) {
            return false;
        }
        if(!isSymmetric(tree1.right, tree2.left)) {
            return false;
        }
        return true;
    }
}
```

* 非递归实现，还是遍历比较根节点的左右子树。采用前序遍历并比较。注意镜像树的操作相反。

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null) {
            return true;
        }
        return isSymmetric(root.left, root.right);
    }

    private boolean isSymmetric(TreeNode tree1, TreeNode tree2) {
        Stack<TreeNode> stack1 = new Stack<>();
        Stack<TreeNode> stack2 = new Stack<>();
        stack1.push(tree1);
        stack2.push(tree2);
        while(!stack1.isEmpty()) {
            // 比较当前结点
            TreeNode cur1 = stack1.pop();
            TreeNode cur2 = stack2.pop();
            if(cur1 == null || cur2 == null) {
                if(cur1 != cur2) {
                    return false;
                }
                continue;
            }
            if(cur1.val != cur2.val) {
                return false;
            }
            // 比较右子树与镜像的左子树
            stack1.push(cur1.right);
            stack2.push(cur2.left);
            // 比较左子树和镜像的右子树
            stack1.push(cur1.left);
            stack2.push(cur2.right);
        }
        return true;
    }
}
```

## 性能分析

&emsp;递归实现时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.5MB，在所有java提交中击败了73.01%的用户。

&emsp;非递归实现时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了10.75%的用户。

内存消耗：38.3MB，在所有java提交中击败了9.16%的用户。

## 官方解题

&emsp;思路与上述相同。