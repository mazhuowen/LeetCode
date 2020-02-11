[toc]

Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.



## 题目解读

&emsp;给定二叉树，给出数右侧的投影。

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
    public List<Integer> rightSideView(TreeNode root) {
        
    }
}
```

## 程序设计

* 实质上是层次遍历取每层的最后一个结点，组成投影。

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()) {
            int len = queue.size();
            // 记录同层最后一个结点
            TreeNode cur = queue.peek();
            for(int i = 0; i < len; i++) {
                cur = queue.poll();
                // 添加下一层的结点
                if(cur.left != null) {
                    queue.add(cur.left);
                }
                if(cur.right != null) {
                    queue.add(cur.right);
                }
            }
            // 加入
            res.add(cur.val);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.8MB，在所有java提交中击败了5.19%的用户。

## 官方解题

&emsp;思路同上。