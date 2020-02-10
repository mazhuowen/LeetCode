[toc]

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

Note: A leaf is a node with no children.



## 题目解读

&emsp;找出所有根节点到叶节点路径之和等于给定树的结点组合。

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
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        
    }
}
```

## 程序设计

* 最简单的，在分递归前序遍历实现的基础上，每个结点保存记录路径的链表。

```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        Stack<Object[]> stack = new Stack<>();
        List<Integer> record = new LinkedList<>();
        record.add(root.val);
        // 新增记录路径的链表
        stack.push(new Object[]{root, root.val, record});
        while(!stack.isEmpty()) {
            Object[] cur = stack.pop();
            TreeNode node = (TreeNode)cur[0];
            int curSum = (Integer)cur[1];
            record = (List<Integer>)cur[2];

            // 判断叶节点
            if(node.left == null && node.right == null && sum == curSum) {
                res.add(record);
            }
            if(node.right != null) {
                List<Integer> temp = new LinkedList<>(record);
                temp.add(node.right.val);
                stack.push(new Object[]{node.right, node.right.val + curSum, temp});
            }
            if(node.left != null) {
                List<Integer> temp = new LinkedList<>(record);
                temp.add(node.left.val);
                stack.push(new Object[]{node.left, node.left.val + curSum, temp});
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N^2)$。

执行用时：8ms，在所有java提交中击败了5.13%的用户。

内存消耗：37.6MB，在所有java提交中击败了79.35%的用户。

## 官方解题

&emsp;暂无，密切关注。社区较通用思路使用递归。

```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        pathSum(root, sum, new LinkedList<>(), res);
        return res;
    }

    private void pathSum(TreeNode root, int sum, List<Integer> record, List<List<Integer>> res) {
        if(root == null) {
            return;
        }
        record.add(root.val);
        // 叶结点且和相等
        if(root.left == null && root.right == null && root.val == sum) {
            // 注意需要copy一份record
            res.add(new LinkedList<>(record));
        } 
        // 不是叶节点或不相等，递归
        else {
            pathSum(root.left, sum - root.val, record, res);
            pathSum(root.right, sum - root.val, record, res);
        }
        // 当前结点遍历完，从链表中移除
        record.remove(record.size()-1);
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏情况为$O(N)$。

执行用时：2ms，在所有java提交中击败了56.97%的用户。

内存消耗：37.5MB，在所有java提交中击败了82.49%的用户。