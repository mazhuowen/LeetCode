[toc]

Given a binary tree **where every node has a unique value**, and a target key `k`, find the value of the nearest leaf node to target k in the tree.

Here, nearest to a leaf means the least number of edges travelled on the binary tree to reach any leaf of the tree. Also, a node is called a leaf if it has no children.



Note:

* `root` represents a binary tree with at least 1 node and at most `1000` nodes.
* Every node has a unique `node.val` in range `[1, 1000]`.
* There exists some node in the given binary tree for which `node.val == k`.



## 题目解读

&emsp;找到离结点`k`的最近的叶节点。

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
    public int findClosestLeaf(TreeNode root, int k) {

    }
}
```

## 程序设计

* 主要的思路为`k`将树划分为两部分，一部分是`k`的子树，叶节点到`k`的距离就是题目要求的距离；另一部分是不包含`k`及`k`的子树的部分，其叶节点到`k`的距离为叶节点到公共最近父节点的距离加`k`到公共最近父节点的距离。
* 可以在一次后序遍历中统计。遍历返回数组，第一位表示叶节点值，第二位表示截止目前结点的路径长度，第三位表示路径是否包含`k`结点。后续遍历，当前结点不是`k`结点，且子路径中都不包含`k`结点，也就是第三位为0，此时只需要选择最小的那条路径并继续返回；如果自路径包含`k`结点，则计算左右子路径之和并更新距离；当当前结点是`k`结点，首先选择较小的子路径，更新距离，然后将计数重置为0，第三位标识为1，标识当前路径从`k`出发。

```java
class Solution {
    int minDis = Integer.MAX_VALUE;
    int node = 0;

    public int findClosestLeaf(TreeNode root, int k) {
        if (root == null) throw new IllegalArgumentException("invalid param");
        postOrder(root, k);
        return node;
    }

    private int[] postOrder(TreeNode root, int k) {
        // 叶节点
        if (root.left == null && root.right == null) {
            if (root.val == k) {
                minDis = 0;
                node = root.val;
                return new int[]{root.val, 0, 1};
            } else {
                return new int[]{root.val, 1, 0};
            }
        }

        int[] left = null, right = null;
        if (root.left != null) {
            left = postOrder(root.left, k);
        }
        if (root.right != null) {
            right = postOrder(root.right, k);
        }
        
        // 当前结点为k
        if (root.val == k) {
            if (left == null && right[1] < minDis) {
                minDis = right[1];
                node = right[0];
            } else if (right == null && left[1] < minDis) {
                minDis = left[1];
                node = left[0];
            } else if (left != null && right != null) {
                if (left[1] <= right[1] && minDis > left[1]) {
                    minDis = left[1];
                    node = left[0];
                } else if (right[1] <= left[1] && minDis > right[1]) {
                    minDis = right[1];
                    node = right[0];
                }
            }
            // 从k重新出发
            return new int[]{0, 0, 1};
        } else {
            if (left == null) {
                right[1]++;
                return right;
            } else if (right == null) {
                left[1]++;
                return left;
            } 
            // 如果有一个子路径包含k，则计算另一条路径叶节点到k的距离，即两者之和加一
            else if (left[2] == 1) {
                if (left[1] + right[1] + 1 < minDis) {
                    minDis = left[1] + right[1] + 1;
                    node = right[0];
                }
                left[1]++;
                return left;
            } 
            // 如果有一个子路径包含k，则计算另一条路径叶节点到k的距离，即两者之和加一
            else if (right[2] == 1) {
                if (left[1] + right[1] + 1 < minDis) {
                    minDis = left[1] + right[1] + 1;
                    node = left[0];
                }
                right[1]++;
                return right;
            } 
            // 都不包含，选择最小的那条
            else {
                if (left[1] < right[1]) {
                    left[1]++;
                    return left;
                } else {
                    right[1]++;
                    return right;
                }
            }
        }
    }
}
```

## 性能分析

&emsp;后序遍历时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.6MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;官方提供思路将树转为图，然后以`k`为起点广度优先搜索，还提出采用额外空间记录路径点来判断更新。

