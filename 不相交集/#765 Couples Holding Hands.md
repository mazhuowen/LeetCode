[toc]

N couples sit in 2N seats arranged in a row and want to hold hands. We want to know the minimum number of swaps so that every couple is sitting side by side. A swap consists of choosing **any** two people, then they stand up and switch seats.

The people and seats are represented by an integer from `0` to `2N-1`, the couples are numbered in order, the first couple being `(0, 1)`, the second couple being `(2, 3)`, and so on with the last couple being `(2N-2, 2N-1)`.

The couples' initial seating is given by `row[i]` being the value of the person who is initially sitting in the i-th seat.



**Note**:

* `len(row)` is even and in the range of `[4, 60]`.
* `row` is guaranteed to be a permutation of `0...len(row)-1`.



## 题目解读

&emsp;$N$对情侣坐在连续排列的$2N$个座位上，想要牵到对方的手。计算最少交换座位的次数，以便每对情侣可以并肩坐在一起。

```java
class Solution {
    public int minSwapsCouples(int[] row) {

    }
}
```

## 程序设计

* 每一对合法的情侣都是连续的两个数字，且偶数数字较小。对于每个不符合要求的对，首先将它们加入集合，然后将应该正确的组合加入集合，即偶数和其后继奇数。最终会形成若干不相交集。而最终的不相交集必须是两两一对，每次最优交换会分裂出一个集合出来，即最后的交换次数为最终不相交集合数减去最初的不相交集数。

* 官方解题借鉴快乐交换猜想，猜想的内容是，如果一次交换之后，没有更多的人变开心，那这次交换一定不在最优解里面。每次将一个沙发上凑成一对情侣之后，在图上的变化是多了一个自循环的连通分量。目标是让图中有$N$个连通分量，每个连通分量代表一对情侣。每次交换都会将连通分量的数量增加1，根据 快乐交换理论，问题的答案可以通过$N$减去最开始情侣图中连通分量的个数来得到。
* 由于一次交换不可能增加多于一个连通分量，所以每次增加一个连通分量的交换就是最优解。

<img src="../images/#765.png" style="zoom: 80%;" />

```java
class Solution {
    public int minSwapsCouples(int[] row) {
       if(row == null || row.length < 2) {
           return 0;
       }
       if(row.length % 2 == 1) throw new IllegalArgumentException("invalid array length");
       int pair = row.length / 2;
       DisJoint disJoint = new DisJoint(row.length);
       for(int i = 0; i < row.length; i += 2) {
           // 集合加入当前对
           disJoint.union(disJoint.find(row[i]), disJoint.find(row[i + 1]));
           // 偶数则加入其后继奇数
           // 如果当前对有效，如2,3，则此处加入的还是2,3；如果无效，如1,2，则会加入3，表示需要和包含3的对做一次交换
           // 如果是0,4这种个情况，则会加入1,5表示对包含1或5的对做两次交换，对同时包含1、5的对做一次交换
           if(row[i] % 2 == 0) {
               disJoint.union(disJoint.find(row[i]), disJoint.find(row[i] + 1));
           }
           if(row[i + 1] % 2 == 0) {
               disJoint.union(disJoint.find(row[i + 1]), disJoint.find(row[i + 1] + 1));
           }
       }
       // 最后的每个不相交集表示交换情侣的连锁操作，这个集合的内部元素可以形成有效情侣对。如包含0,1,4,5的集合，最终需要形成的集合是0,1;4,5
       // 而最优交换可以每次将不相交集分裂出来一个，则最优操作数就是最后的集合数目减去初始的集合数目
       return pair - disJoint.size();
    }
}

class DisJoint {
    private int size;
    private int[] tree;

    DisJoint(int size) {
        this.size = size;
        this.tree = new int[size];
        // 初始化为size个不相交集
        for(int i = 0; i < size; i++) {
            tree[i] = -1;
        }
    }
    // 按规模并
    public void union(int root1, int root2) {
        if(tree[root1] >= 0 || tree[root2] >= 0) {
            throw new IllegalArgumentException("not a root");
        }
        if(root1 == root2) {
            return;
        }
        // 树2大，树1并入树2
        if(tree[root1] >= tree[root2]) {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        } else {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        }
        size--;
    }
    // 路径压缩
    public int find(int val) {
        if(tree[val] < 0) {
            return val;
        }
        return tree[val] = find(tree[val]);
    }

    public int size() {
        return size;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.8MB，在所有java提交中击败了5.97%的用户。

## 官方解题

&emsp;官方使用采用图的方法寻找连通分量。其次官方还提供了贪心算法，其思路出发点为对于每张沙发，找到沙发上第一个人的情侣，如果不在同一个沙发上，就把沙发上的第二人换成第一个人的情侣。根据上面的假设，每次交换都可以增加一对开心的人，这样交换是最优的。此处使用了小技巧，即情侣1为`X`，则情侣2为`X^1`。

```java
class Solution {
    public int minSwapsCouples(int[] row) {
        int ans = 0;
        for (int i = 0; i < row.length; i += 2) {
            int x = row[i];
            // 两个数字是连续的，且偶数较小
            if (row[i+1] == (x ^ 1)) continue;
            // 不符合要求，需要交换
            ans++;
            for (int j = i + 2; j < row.length; j++) {
                // 索引j上的数字是i的情侣，交换
                if (row[j] == (x^1)) {
                    row[j] = row[i+1];
                    row[i+1] = x^1;
                    break;
                }
            }
        }
        return ans;
    }
}
```

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.9MB，在所有java提交中击败了5.97%的用户。