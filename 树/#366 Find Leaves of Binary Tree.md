[toc]

Given a binary tree, collect a tree's nodes as if you were doing this: Collect and remove all leaves, repeat until the tree is empty.



## 题目解读

&emsp;给定二叉树，每次删除叶节点，直到删除为空。

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
    public List<List<Integer>> findLeaves(TreeNode root) {
        
    }
}
```

## 程序设计

* 我们直到结点到叶结点的最短长度为结点的层数，如果定义到叶结点的最长长度为几点的层数，仔细观察结点的删除方式，第一批删除结点是叶节点，层数是0，放在链表索引为0的位置；第二批删除的结点层数是1，放在链表索引为1的位置，以此类推，直到根结点。
* 注意删除顺序是从左到右的顺序，在递归时先遍历左子节点，后遍历右子节点。实质是后序遍历。

```java
class Solution {
    public List<List<Integer>> findLeaves(TreeNode root) {
        List<List<Integer>> res = new LinkedList<>();
        if(root == null) {
            return res;
        }
        findLeaves(root, res);
        return res;
    }

    private int findLeaves(TreeNode root, List<List<Integer>> res) {
        if(root == null) {
            return -1;
        }
        // 递归遍历
        int leftLevel = findLeaves(root.left, res);
        int rightLevel = findLeaves(root.right, res);
        // 当前结点距离叶节点的最大层数（即最后删除的顺序）
        int level = Math.max(leftLevel, rightLevel) + 1;
        // 创建list
        if(res.size() == level || res.get(level) == null) {
                res.add(new LinkedList<>());
        }
        res.get(level).add(root.val);
        // 返回当前结点的层数
        return level;
    }
}
```

## 性能分析

&emsp;递归方法时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.6MB，在所有java提交中击败了6.10%的用户。

## 官方解题

&emsp;暂无，密切关注。