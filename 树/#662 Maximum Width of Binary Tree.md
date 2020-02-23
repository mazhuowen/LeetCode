Given a binary tree, write a function to get the maximum width of the given tree. The width of a tree is the maximum width among all levels. The binary tree has the same structure as a full binary tree, but some nodes are null.

The width of one level is defined as the length between the end-nodes (the leftmost and right most non-null nodes in the level, where the null nodes between the end-nodes are also counted into the length calculation.

**Note:** Answer will in the range of 32-bit signed integer.



## 题目解读

&emsp;给出二叉树的宽度。

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
    public int widthOfBinaryTree(TreeNode root) {

    }
}
```

## 程序设计

* 观察示例，树的最大宽度可以认为是每层宽度的最大值。问题的难点来到了怎么计算得出每层节点的编号可以用来计算宽度。可以联想到树的顺序表存储，父节点索引为$i$，则左子节点为$2 * i$，右子节点为$2 * i + 1$。利用这个思路进行层次遍历，每遍历一层，计算宽度并更新。

```java
// 辅助类
class Pair {
    TreeNode node;
    int index;

    Pair(TreeNode node, int index) {
        this.node = node;
        this.index = index;
    }
}
class Solution {
    public int widthOfBinaryTree(TreeNode root) {
        int width = 0;
        if(root == null) {
            return width;
        }
        Queue<Pair> queue = new LinkedList<>();
        // 入队根节点
        queue.add(new Pair(root, 1));
        while(!queue.isEmpty()) {
            int len =  queue.size();
            int start = 0;
            for(int i = 0; i < len; i++) {
                Pair cur = queue.poll();
                // 添加子节点
                if(cur.node.left != null) {
                    queue.add(new Pair(cur.node.left, cur.index * 2));
                }
                if(cur.node.right != null) {
                    queue.add(new Pair(cur.node.right, cur.index * 2 + 1));
                }
                // 记录一层开始索引
                if(i == 0) {
                    start = cur.index;
                }
                // 一层结束节点，计算更新宽度
                if(i == len - 1) {
                    width = Math.max(width, cur.index - start + 1);
                }
            }
        }
        return width;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.1MB，在所有java提交中击败了7.84%的用户。

## 官方解题

&emsp;核心思路同上，形式不同。