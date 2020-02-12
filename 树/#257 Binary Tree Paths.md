[toc]

Given a binary tree, return all root-to-leaf paths.

Note: A leaf is a node with no children.



## 题目解读

&emsp;给定二叉树，得到所有根结点到叶节点的链路。

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
    public List<String> binaryTreePaths(TreeNode root) {
        
    }
}
```

## 程序设计

* 实质就是前序遍历，将遍历到的路径保存，直到遇到叶节点，表示路径完成。递归思路如下：

```java
class Solution {
    String path = "";

    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        binaryTreePaths(root, res);
        return res;
    }

    private void binaryTreePaths(TreeNode root, List<String> res) {
        String temp = path;
        path += "->" + root.val;
        // 叶节点返回
        if(root.left == null && root.right == null) {
            String p = path.toString();
            res.add(p.substring(2));
            // 回溯
            path = temp;
            return;
        }
        // 继续递归
        if(root.left != null) {
            binaryTreePaths(root.left, res);
        }
        if(root.right != null) {
            binaryTreePaths(root.right, res);
        }
        // 回溯
        path = temp;
    }
}
```

* 非递归思路如下：

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        Stack<TreeNode> stack = new Stack<>();
        Stack<String> paths = new Stack<>();
        stack.push(root);
        paths.push(root.val + "");
        while(!stack.isEmpty()) {
            TreeNode curNode = stack.pop();
            String path = paths.pop();
            if(curNode.left == null && curNode.right == null) {
                res.add(path);
            }
            if(curNode.right != null) {
                stack.push(curNode.right);
                paths.push(path + "->" + curNode.right.val);
            }
            if(curNode.left != null) {
                stack.push(curNode.left);
                paths.push(path + "->" + curNode.left.val);
            }
        }
        return res;
    }
}
```

* 考虑到字符串每次拼接会新生成，不会改变原来的值，无需回溯；其次拼接数字用`Integer.toString()`可大幅提高运行时间，优化递归如下：

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new LinkedList<>();
        binaryTreePaths(root, "", res);
        return res;
    }

    private void binaryTreePaths(TreeNode root, String path, List<String> res) {
        if(root == null) {
            return;
        }
        path += Integer.toString(root.val);
        if(root.left == null && root.right == null) {
            res.add(path);
        } else {
            path += "->";
            binaryTreePaths(root.left, path, res);
            binaryTreePaths(root.right, path, res);
        }
    }
}
```

## 性能分析

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏为$O(N)$。

执行用时：12ms，在所有java提交中击败了10.73%的用户。

内存消耗：42.9MB，在所有java提交中击败了5.03%的用户。

&emsp;非递归实现时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：22ms，在所有java提交中击败了10.73%的用户。

内存消耗：43MB，在所有java提交中击败了5.03%的用户。

## 官方解题

&emsp;思路同上。