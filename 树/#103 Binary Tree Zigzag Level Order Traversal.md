[toc]

Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).



## 题目解读

&emsp;给定一个二叉树，返回其节点值的锯齿形层次遍历。即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行。

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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        
    }
}
```

## 程序设计

* 在层次遍历的基础上增加是否反序标识。

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        // 是否反向遍历
        boolean zigzag = false;
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
            // 翻转链表
            if(zigzag) {
                Collections.reverse(temp);
            }
            res.add(temp);
            // 更新下一次的标识
            zigzag = !zigzag;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：1ms，在所有java提交中击败了98.78%的用户。

内存消耗：36.3MB，在所有java提交中击败了27.30%的用户。

## 官方解题

&emsp;暂无，密切关注。