[toc]

In an infinite binary tree where every node has two children, the nodes are labelled in row order.

In the odd numbered rows (ie., the first, third, fifth,...), the labelling is left to right, while in the even numbered rows (second, fourth, sixth,...), the labelling is right to left.

<img src="../images/#1104.png" style="zoom: 33%;" />

Given the `label` of a node in this tree, return the labels in the path from the root of the tree to the node with that `label`.



**Constraints:**

- $1 \le \text{label} \le 10^6$



## 题目解读

&emsp;求解锯齿编号节点的路径。

```java
class Solution {
    public List<Integer> pathInZigZagTree(int label) {

    }
}
```

## 程序设计

* 如果完全二叉树节点编号是顺序的，则已知节点值`val`，其父节点值为`val / 2`；如果是锯齿编号，则父节点为`val / 2`在同层的对称节点；其次对于锯齿编号，祖父节点为`val / 4`，根据这两个；规律可以当前节点、父节点两两迭代更新。
* 求解由于同层节点编号是等差数列，对称编号就是同层最大节点值加最小值减去当前值得到。

```java
class Solution {
    public List<Integer> pathInZigZagTree(int label) {
        if (label < 1) throw new IllegalArgumentException("invalid param");
        List<Integer> res = new LinkedList<>();

        int cur = label;
        int parent = getParent(cur);

        while (cur > 0) {
            res.add(0, cur);
            if (parent > 0) res.add(0, parent);
            // 当前节点、父节点两两迭代
            cur /= 4;
            parent /= 4;
        }
        return res;
    }

    private int getParent(int val) {
        // 根节点无父节点
        if (val == 1) return -1;
        int level = getLevel(val);
        // 同层对称节点和
        int levelSum = (1 << (level - 1)) + (1 << level) - 1;
        return levelSum - val / 2;
    }

    // 获取层数
    private int getLevel(int val) {
        int level = 0;
        while (val > 1) {
            val >>= 1;
            level++;
        }
        return level;
    }
}
```

## 性能分析

&emsp;时间复杂度为$\log_2N$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.9MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;暂无，密切关注。