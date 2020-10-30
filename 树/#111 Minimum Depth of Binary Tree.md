[toc]

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note**: A leaf is a node with no children.



## 题目解读

&emsp;给定二叉树，求解树的最小深度。

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
    public int minDepth(TreeNode root) {
        
    }
}
```

## 程序设计

* 最容易的递归实现，当前结点的最小层数是两个子节点最小层数加一。但是注意到题目中做了限制，层数是叶子结点到当前结点，这意味着需要判断当前结点是否都有左右子树，没有则层数是其唯一子节点层数加一而不是0。

```java
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) {
            return 0;
        }
        // 递归左右子树
        int left = minDepth(root.left);
        int right = minDepth(root.right);

        // 需注意题目要求是叶子结点到当前结点的距离
        // 没有左子树，则层数为右子树层数加一
        if(root.left == null) {
            return right + 1;
        }
        // 没有右子树，则层数为左子树层数加一
        if(root.right == null) {
            return left + 1;
        }
        // 否则选择小的值加一
        return left > right ? right + 1 : left + 1;
    }
}
```

> 中间的两个if判断就是为了迎合本题对层数的定义。

* 非递归实现可以使用前序遍历。当当前结点是叶结点时，计算更新最小层数。

```java
import javafx.util.Pair;
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int minDep = Integer.MAX_VALUE;
        Stack<Pair<TreeNode, Integer>> stack = new Stack<>();
        stack.push(new Pair(root, 1));
        while(!stack.isEmpty()) {
            Pair<TreeNode, Integer> cur = stack.pop();
            TreeNode curNode = cur.getKey();
            // 当前结点是叶结点，更新最小深度
            if(curNode.left == null && curNode.right == null) {
                minDep = Math.min(minDep, cur.getValue());
            }
            if(curNode.left != null) {
                stack.push(new Pair(curNode.left, cur.getValue() + 1));
            }
            if(curNode.right != null) {
                stack.push(new Pair(curNode.right, cur.getValue() + 1));
            }
        }

        return minDep;
    }
}
```

## 性能分析

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏情况为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.4MB，在所有java提交中击败了5.07%的用户。

&emsp;非递归方法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：5ms，在所有java提交中击败了35.59%的用户。

内存消耗：39.4MB，在所有java提交中击败了5.07%的用户。

## 官方解题

&emsp;除了上述递归和非递归实现，一个更好的思路是层次遍历，遇到叶结点则直接结束遍历，返回最小值。

```java
import javafx.util.Pair;
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) {
            return 0;
        }
        int minDep = Integer.MAX_VALUE;
        Queue<Pair<TreeNode, Integer>> queue = new LinkedList<>();
        queue.add(new Pair(root, 1));
        while(!queue.isEmpty()) {
            Pair<TreeNode, Integer> cur = queue.poll();
            TreeNode curNode = cur.getKey();
            // 该层检测到叶结点，直接返回
            if(curNode.left == null && curNode.right == null) {
                minDep = cur.getValue();
                break;
            }
            if(curNode.left != null) {
                queue.add(new Pair(curNode.left, cur.getValue() + 1));
            }
            if(curNode.right != null) {
                queue.add(new Pair(curNode.right, cur.getValue() + 1));
            }
        }
        return minDep;
    }
}
```

&emsp;最坏的情况是完全二叉树，需要遍历到最后一层，时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时 :1ms，在所有java提交中击败了35.59%的用户

内存消耗 :39.5MB，在所有java提交中击败了5.07%的用户