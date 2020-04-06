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

* 重构上述代码，使用`Integer`数组，而不是`int`，可以携带更多信息。这样递归终结可以从空结点开始，而非叶节点，省去左右叶节点的判断。空结点则返回`[null,0,0]`；当当前结点获取到左右子节点的值时，分如下情况：

  * 当前结点是k结点，则选择子节点中路径最短的，并与最短距离比较更新，如果当前结点是叶节点，则距离为0；关键一点是返回距离重置为0的数组，表示以k为根的子树的距离已统计完成，从k结点开始统计计算到祖先结点的距离。如果是叶节点则返回`[k,0,1]`；如果不是叶节点，则返回`[null, 0, 1]`；

  * 当前结点不是k结点，且子节点路径中不包含k结点，则只需选择距离最短的并返回；

  * 当前结点不是k结点，子路径中包含k结点，则计算另一分支与包含k结点分支的距离，并更新，而返回只包含k结点的路径。

```java
class Solution {
    int minDis = Integer.MAX_VALUE;
    int node = 0;

    public int findClosestLeaf(TreeNode root, int k) {
        if (root == null) throw new IllegalArgumentException("invalid param");
        postOrder(root, k);
        return node;
    }

    private Integer[] postOrder(TreeNode root, int k) {
        if (root == null) return new Integer[]{null, 0, 0};

        Integer[] left = postOrder(root.left, k);
        Integer[] right = postOrder(root.right, k);

        // 叶节点是k，则距离是0，已经是最短距离，直接返回
        if ((left[0] != null && left[0] == k) || (right[0] != null && right[0] == k)) return left[0] != null && left[0] == k ? left : right;

        Integer[] res;
        // 当前结点是k结点
        if (root.val == k) {
            res = getMinPath(left, right);
            // 说明是叶节点
            if (res[0] == null) {
                minDis = 0;
                node = k;
                res = new Integer[]{k, 0, 1};
            } else {
                if (minDis > res[1] + 1) {
                    minDis = res[1] + 1;
                    node = res[0];
                }
                // 重置k点的距离，返回从k出发到祖先结点的距离
                res = new Integer[]{null, 0, 1};
            }
        }
        // 子路径存在k结点，则计算更新并返回带k的路径
        else if (left[2] == 1) {
            if (right[0] != null && right[1] + left[1] + 2 < minDis) {
                minDis = right[1] + left[1] + 2;
                node = right[0];
            }
            res = left;
            res[1] += 1;
        } 
        else if (right[2] == 1) {
            if (left[0] != null && right[1] + left[1] + 2 < minDis) {
                minDis = right[1] + left[1] + 2;
                node = left[0];
            } 
            res = right;
            res[1] += 1;
        }
        // 子路径不带有k结点
        else {
            res = getMinPath(left, right);
            // 是叶节点
            if (res[0] == null) {
                res = new Integer[]{root.val, 0, 0};
            } 
            // 非叶节点，返回较小的路径
            else {
                res[1] += 1;
            }
        }

        return res;
    }

    // 返回距离较小的子节点
    private Integer[] getMinPath(Integer[] left, Integer[] right) {
        if (left[0] == null) return right;
        else if (right[0] == null) return left;

        return left[1] < right[1] ? left : right;
    }
}
```

## 性能分析

&emsp;后序遍历时间复杂度为$O(N)$，空间复杂度为$O(\log_2N)$，最坏$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.6MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;官方提供思路将树转为图，然后以`k`为起点广度优先搜索，还提出采用额外空间记录路径点来判断更新。

