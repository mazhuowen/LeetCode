[toc]

Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).



## 题目解读

&emsp;给定二叉树，输出从底层到根节点的层次遍历。

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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        
    }
}
```

## 程序设计

* 可直接参考层次遍历，最后将结果翻转即可。

```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()) {
            List<Integer> temp = new LinkedList<>();
            int len = queue.size();
            for(int i = 0; i < len; i++) {
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
        // 翻转链表
        Collections.reverse(res);
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.6MB，在所有java提交中击败了13.50%的用户。

## 官方解题

&emsp;暂无，密切关注。