[toc]

Given a binary tree containing digits from `0-9` only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path `1->2->3` which represents the number `123`.

Find the total sum of all root-to-leaf numbers.

**Note**: A leaf is a node with no children.



## 题目解读

&emsp;给定二叉树，每条从根到叶节点的路径代表一个数字，需要求解二叉树表示的所有数字之和。

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
    public int sumNumbers(TreeNode root) {
        
    }
}
```

## 程序设计

* 可以使用递归方式，每遍历一个结点，更新当前数字，当遍历到叶节点时更新和。

```java
class Solution {
    private int sum = 0;
    private int num = 0;

    public int sumNumbers(TreeNode root) {
        if(root == null) {
            return sum;
        }
        numbers(root);
        return sum;
    }

    private void numbers(TreeNode root) {
        // 更新数字
        num = num * 10 + root.val;
        // 叶结点更新和
        if(root.left == null && root.right == null) {
            sum += num;
        }
        if(root.left != null) {
            numbers(root.left);
        }
        if(root.right != null) {
            numbers(root.right);
        }
        // 非常重要，回溯到前一个值
        num /= 10;
    }
}
```

* 非递归实现可以使用前序遍历思路来实现，当遍历到叶结点，更新和。

```java
class Pair {
    TreeNode node;
    int sum;

    Pair(TreeNode node, int sum) {
        this.node = node;
        this.sum = sum;
    }
}
class Solution {
    public int sumNumbers(TreeNode root) {
        int sum = 0;
        if(root == null) {
            return sum;
        }
        Stack<Pair> stack = new Stack<>();
        stack.push(new Pair(root, root.val));
        while(!stack.isEmpty()) {
            Pair cur = stack.pop();
            TreeNode curNode = cur.node;
            // 是叶节点，则更新和
            if(curNode.left == null && curNode.right == null) {
                sum += cur.sum;
            }
            // 入栈
            if(curNode.right != null) {
                stack.push(new Pair(curNode.right, cur.sum * 10 + curNode.right.val));
            }
            if(curNode.left != null) {
                stack.push(new Pair(curNode.left, cur.sum * 10 + curNode.left.val));
            }
        }
        return sum;
    }
}
```

## 性能分析

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.4MB，在所有java提交中击败了5.00%的用户。

&emsp;非递归方法时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了19.47%的用户。

内存消耗：40.7MB，在所有java提交中击败了5.00%的用户。

## 官方解题

&emsp;暂无，密切关注。