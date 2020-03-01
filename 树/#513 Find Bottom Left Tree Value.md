[toc]

Given a binary tree, find the leftmost value in the last row of the tree.

**Note:** You may assume the tree (i.e., the given root node) is not `null`.



## 题目解读

&emsp;给出最后一层最左边的叶节点。

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
    public int findBottomLeftValue(TreeNode root) {
        
    }
}
```

## 程序设计

* 最直观的是层次遍历最后一层第一个结点。

```java
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        // 记录第一个结点
        TreeNode last = root;
        while(!queue.isEmpty()) {
            int len = queue.size();
            for(int i = 0; i < len; i++) {
                TreeNode cur = queue.poll();
                if(i == 0) {
                    last = cur;
                }
                if(cur.left != null) {
                    queue.add(cur.left);
                }
                if(cur.right != null) {
                    queue.add(cur.right);
                }
            }   
        }
        return last.val;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了69.86%的用户。

内存消耗：41.5MB，在所有java提交中击败了5.09%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用前序遍历的思路，记录深度，返回最大的值。

```java
class Solution {
    private int max;
    private TreeNode last;

    public int findBottomLeftValue(TreeNode root) {
        preorder(root, 1);
        return last.val;
    }

    private void preorder(TreeNode root, int depth) {
        if(root == null) {
            return;
        }
        // 大于则更新（由于遍历先左后右，保证是最大的第一个结点）
        if(depth > max) {
            max = depth;
            last = root;
        }
        preorder(root.left, depth + 1);
        preorder(root.right, depth + 1);
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.5MB，在所有java提交中击败了7.40%的用户。