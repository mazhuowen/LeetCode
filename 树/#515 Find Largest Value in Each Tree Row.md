[toc]

You need to find the largest value in each row of a binary tree.



## 题目解读

&emsp;返回树每层最大的值。

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
    public List<Integer> largestValues(TreeNode root) {

    }
}
```

## 程序设计

* 最直观的，层次遍历。

```java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()) {
            int len = queue.size();
            int max = Integer.MIN_VALUE;
            for(int i = 0; i < len; i++) {
                TreeNode cur = queue.poll();
                max = Math.max(max, cur.val);
                if(cur.left != null) {
                    queue.add(cur.left);
                }
                if(cur.right != null) {
                    queue.add(cur.right);
                }
            }
            res.add(max);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了82.36%的用户。

内存消耗：42.4MB，在所有java提交中击败了5.06%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还有使用前序遍历思路，记录每个结点的层数并做判断的思路。