[toc]

Given a binary tree, return the vertical order traversal of its nodes values.

For each node at position `(X, Y)`, its left and right children respectively will be at positions `(X-1, Y-1)` and `(X+1, Y-1)`.

Running a vertical line from `X = -infinity` to `X = +infinity`, whenever the vertical line touches some nodes, we report the values of the nodes in order from top to bottom (decreasing `Y` coordinates).

If two nodes have the same position, then the value of the node that is reported first is the value that is smaller.

Return an list of non-empty reports in order of `X` coordinate.  Every report will have a list of values of nodes.



Note:

* The tree will have between $1$ and $1000$ nodes.
* Each node's value will be between $0$ and $1000$.



## 题目解读

&emsp;垂直遍历树，题目要求如果多个结点坐标一致，则选择结点值小的在前面。

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
    public List<List<Integer>> verticalTraversal(TreeNode root) {

    }
}
```

## 程序设计

* 由使用层次遍历，需要注意坐标一致的情况，此时需要选择较小的值先遍历，其次入队需要先将所有一致结点的左子节点入队，后在入队右子节点。

```java
class Solution {

    public List<List<Integer>> verticalTraversal(TreeNode root) {
        LinkedList<List<Integer>> res = new LinkedList<>();
        if (root == null) return res;

        int offset = 0;
        Queue<Pair> queue = new LinkedList<>();
        queue.add(new Pair(root, 0));

        while (!queue.isEmpty()) {
            int size = queue.size();
            while (size > 0){
                // 当前层坐标
                int idx = queue.peek().idx;
                LinkedList<TreeNode> temp = new LinkedList<>();
                // 坐标相同，将这些结点加入链表
                while (size > 0 && queue.peek().idx == idx) {
                    size--;
                    temp.add(queue.poll().node);
                }
                // 对坐标相同的点进行排序，值小的在前面
                if (temp.size() > 1) temp.sort((a, b) -> a.val - b.val);
                
                if (idx + offset >= 0) {
                    if (idx + offset >= res.size() || res.get(idx + offset) == null)
                        res.addLast(new LinkedList<>());
                } else {
                    res.addFirst(new LinkedList<>());
                    offset++;
                }
                // 依次加入遍历链表
                for (TreeNode cur : temp) res.get(idx + offset).add(cur.val);
                // 加入队列，先左子节点，后右子节点
                for (TreeNode cur : temp) if (cur.left != null) queue.add(new Pair(cur.left, idx - 1));
                for (TreeNode cur : temp) if (cur.right != null) queue.add(new Pair(cur.right, idx + 1));
            }
        }
        return res;
    }
}

class Pair {
    TreeNode node;
    int idx;

    public Pair(TreeNode node, int idx) {
        this.node = node;
        this.idx = idx;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了62.50%的用户。

内存消耗：39.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方思路采用遍历记录结点坐标，并插入链表，最后再排序。