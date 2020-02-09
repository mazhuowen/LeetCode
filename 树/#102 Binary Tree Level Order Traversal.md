[toc]

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).



## 题目解读

&emsp;给定二叉树，进行层次遍历。

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        
    }
}
```

## 程序设计

* 与普通层次遍历不同，题目要求输出为每层结点值的列表，需要改变部分思路。当每一层的结点都入队后再出队，而不是一个个出队。

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()) {
            // 每一层创建链表
            List<Integer> temp = new LinkedList<>();
            int levelLen = queue.size();
            // 将该层结点出队，同时把下一层结点入队
            for(int i = 0; i < levelLen; i++) {
                TreeNode cur = queue.poll();
                temp.add(cur.val);
                if(cur.left != null) {
                    queue.add(cur.left);
                }
                if(cur.right != null) {
                    queue.add(cur.right);
                }
            }
            res.add(temp);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：1ms，在所有java提交中击败了98.65%的用户。

内存消耗：36.5MB，在所有java提交中击败了13.53%的用户。

## 官方解题

&emsp;官方解题提供了递归思路

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        // 递归
        levelOrder(root, 0, res);
        return res;
    }

    private void levelOrder(TreeNode root, int level, List<List<Integer>> res) {
        // 创建该层链表
        if(level == res.size()) {
            res.add(new LinkedList<Integer>());
        }
        // 加入该层
        res.get(level).add(root.val);
        // 递归
        if(root.left != null) {
            levelOrder(root.left, level + 1, res);
        }
        if(root.right != null) {
            levelOrder(root.right, level + 1, res);
        }
    }
}
```

&emsp;时间复杂度$O(N)$，空间复杂度$O(\log_2N)$，最坏情况空间复杂度$O(N)$。

执行用时：1ms，在所有java提交中击败了98.65%的用户。

内存消耗：36.2MB，在所有java提交中击败了77.96%的用户。