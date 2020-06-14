[toc]

Given integers `n` and `k`, find the lexicographically k-th smallest integer in the range from `1` to `n`.

Note: $1 \le k \le n \le 10^9$.



## 题目解读

&emsp;找到要求字典序的数字。

```java
class Solution {
    public int findKthNumber(int n, int k) {

    }
}
```

## 程序设计

* 将问题抽象为多叉树，即根节点由$1 \sim 9$组成的$9$棵树森林，子结点均由$0 \sim 9$十个组成，这样森林的每一层数字都是连续的，每个节点下一层的第一个节点是当前节点值乘$10$；
* 有了上述的抽象，问题转化为统计当根为前节点的子树有多少个节点，根据节点数判断是否在当前子树，统计节点可由同层数字连续来计算。

```java
class Solution {
    public int findKthNumber(int n, int k) {
        if (n < 1 || n < k) throw new IllegalArgumentException("invalid param");

        // 遍历根节点为1～9的森林，判断是否是第k个
        int root = 1;

        while (k > 0) {
            int count = getCount(root, n);
            // 当前数目不够，需要迭代到下一棵树中，根节点加一
            if (count < k) {
                k -= count;
                root++;
            } 
            // 要求的数在当前子树中，遍历下一层，从第一个节点继续求解
            else {
                // 当前根节点不是要求的点，减一
                k--;
                root *= 10;
            }
        }
        // k--为0时，root多乘了10，需要除去
        return root / 10;
    }

    private int getCount(int root, int limit) {
        int count = 0;
        // 根和后继根，乘10相当于遍历到左节点（注意数据溢出）
        long left = root, right = root + 1;
        while (left <= limit) {
            // 计算当前层节点数，同层左节点相减得到节点数
            count += Math.min(right, limit + 1) - left;
            // 迭代到下一层
            left *= 10;
            right *= 10;
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_{10}N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.4MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。