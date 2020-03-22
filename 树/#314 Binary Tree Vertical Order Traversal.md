[toc]

Given a binary tree, return the *vertical order* traversal of its nodes' values. (ie, from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from **left to right**.



## 题目解读

&emsp;给定二叉树，题目要求给出“垂直遍历”的序列。题目要求每一列的顺序为从高到低，从左到右。

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
    public List<List<Integer>> verticalOrder(TreeNode root) {

    }
}
```

## 程序设计

* 仔细观察示例，题目定义每一列的结点，假设当前结点列为`x`，则其左结点为`x - 1`，右结点为`x + 1`，有了这个规律可以将根节点的相对列设为0遍历。
* 另一个问题是最后结果的序列顺序。首先链表按照列的大小排列，上面我们使用相对列计算，肯定存在负值，我们可以利用集合记录已经遍历的列，对于未遍历的列，采用正数列拼接到数组后面，负数列拼接到数组前面。因为未遍历的负数列其前的列必然已遍历，故插入到数组前面不会造成列的不连续，正数列同理。
* 另一个问题是索引因为插入而发生变化。初始时，列就是数组的索引，随着负数列的插入，正数列后移，我们可以记录这个偏移，最后每一列在数组中的索引就是：`相对列值 + 偏移值`。
* 除了外层链表的顺序，观察内部链表的顺序，发现遵循自顶向下，从左至右的顺序，这似乎是前序遍历的顺序，但是本题其它结点或子树的分之可能和当前结点在同一行，同一列，这使得前序遍历无法满足条件，因为不能保证全局的顺序只能保证子树的顺序。一个更好的方案是层次遍历，这样全局的顺序得到保证。

```java
class Solution {
    public List<List<Integer>> verticalOrder(TreeNode root) {
        List<List<Integer>> res = new LinkedList<>();
        if (root == null) return res;

        // 层次遍历，初始化根节点为第0列（参照列）
        Queue<TreeNode> queue = new LinkedList<>(); // 记录结点
        Queue<Integer> colnum = new LinkedList<>();// 记录相对列
        queue.add(root);
        colnum.add(0); 

        // 记录偏移值
        int offset = 0;
        Set<Integer> set = new HashSet<>();
        while (!queue.isEmpty()) {
            TreeNode cur = queue.poll();
            int col = colnum.poll();
            // 第一次访问该列
            if (!set.contains(col)) {
                // 正数列插入后面，偏移值不变
                if (col >= 0) {
                    res.add(new LinkedList<>());
                } 
                // 负数列插入前面，偏移值加一
                else {
                    res.add(0, new LinkedList<>());
                    offset++;
                }
                // 已访问集合加入列
                set.add(col);
            }
            // 根据偏移和相对列值获取数组，并拼接到后面
            res.get(offset + col).add(cur.val);
            // 加入下一层结点
            if (cur.left != null) {
                queue.add(cur.left);
                colnum.add(col - 1);
            }
            if (cur.right != null) {
                queue.add(cur.right);
                colnum.add(col + 1);
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：2ms，在所有java提交中击败了100.00%的用户。

内存消耗：38.9MB，在所有java提交中击败了8.70%的用户。

## 官方解题

&emsp;暂无，密切关注。

