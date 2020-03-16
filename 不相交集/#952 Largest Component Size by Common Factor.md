[toc]

Given a non-empty array of unique positive integers `A`, consider the following graph:

- There are `A.length` nodes, labelled `A[0]` to `A[A.length - 1];`
- There is an edge between `A[i]` and `A[j]` if and only if `A[i]` and `A[j]` share a common factor greater than 1.

Return the size of the largest connected component in the graph.



**Note:**

* $1 \le \text{A.length} \le 20000$
* $1 \le \text{A[i]} \le 100000$



## 题目解读

&emsp;给定正整数的数组，每个数之间如果存在公因数大于1，则表示索引表示的两个点之间存在边。给定构成的图的最大连通分量。

```java
class Solution {
    public int largestComponentSize(int[] A) {

    }
}
```

## 程序设计

* 第一想法是利用最大公倍数求解的值大于1表示存在公因数，而后利用不相交集关联。最后得到最大的集合尺寸。时间复杂度为$O(N^2)$，空间复杂度为$O(N)$，但是会超时。

```java
class Solution {
    public int largestComponentSize(int[] A) {
        if(A == null || A.length == 0) {
            return 0;
        }
        DisJoint disJoint = new DisJoint(A.length);
        for(int i = 0; i < A.length - 1; i++) {
            for(int j = i + 1; j < A.length; j++) {
                // 如果最大公约数大于1，则必然存在公因数
                if(getLcm(A[i], A[j]) > 1) {
                    // 关联
                    disJoint.union(disJoint.find(i), disJoint.find(j));
                }
            }
        }
        // 返回最大的连通分量
        return disJoint.getMaxSet();
    }

    private int getLcm(int num1, int num2) {
        if(num1 == 0) return num2;
        return getLcm(num2 % num1, num1);
    }
}

class DisJoint {
    private int size;
    private int[] tree;
    // 记录当前不相交集中的最大值
    private int maxSet;

    DisJoint(int size) {
        this.size = size;
        this.tree = new int[size];
        this.maxSet = 1;
        Arrays.fill(tree, -1);
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
            maxSet = Math.max(maxSet, -tree[root2]);
        } else {
            tree[root1] += tree[root2];
            tree[root2] = root1;
            maxSet = Math.max(maxSet, -tree[root1]);
        }
        size--;
    }
    // 路径压缩
    public int find(int val) {
        if(tree[val] < 0) return val;
        return tree[val] = find(tree[val]);
    }

    public int size() {
        return size;
    }

    public int getMaxSet() {
        return maxSet;
    }
}
```

* 注意到数组规模达到20000，平方级别的方法必然不行。转换思路，如果计算每个数的所有质数，将质数作为关联连接两个数字。于是得到如下代码，由于集合中包含了数组中不存在的质数，故需要重新遍历计数。但是该方法仍然超时。

```java
class Solution {
    public int largestComponentSize(int[] A) {
        if(A == null || A.length == 0) {
            return 0;
        }

        DisJoint disJoint = new DisJoint(100_001);
        for(int num : A) {
            // 计算质因数
            int prime = 2, temp = num;
            while(prime <= temp) {
                // 是一个因数
                if(temp % prime == 0) {
                    // 将质因数和当前数关联
                    disJoint.union(disJoint.find(prime), disJoint.find(num));
                    // 将该因数除尽（很关键，保证了迭代得到的数是质数）
                    while (temp % prime == 0) temp /= prime;
                }
                prime++;
            }
        }
        int max = 1;
        // 计数不相交集和集合尺寸
        int[] counter = new int[100_001];
        for (int i = 0; i < A.length; i++) {
            max = Math.max(max, ++counter[disJoint.find(A[i])]);
        }
        return max;
    }
}
```

* 上述思路求解质数导致超时，可以不用求解所有质数，而是将除数都加入集合，这样大大减少求解时间。

```java
class Solution {
    public int largestComponentSize(int[] A) {
        if(A == null || A.length == 0) {
            return 0;
        }

        DisJoint disJoint = new DisJoint(100_001);
        for(int num : A) {
            for (int prime = (int)Math.sqrt(num); prime >= 2; prime--) {
                if(num % prime == 0) {
                    disJoint.union(disJoint.find(prime), disJoint.find(num));
                    disJoint.union(disJoint.find(num / prime), disJoint.find(num));
                }
            }
        }
        int max = 1;
        // 计数不相交集和集合尺寸
        int[] counter = new int[100_001];
        for (int i = 0; i < A.length; i++) {
            max = Math.max(max, ++counter[disJoint.find(A[i])]);
        }
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\sqrt{M})$，空间复杂度为$O(N)$，其中$M$为最大数字值。

执行用时：137ms，在所有java提交中击败了81.94%的用户。

内存消耗：42MB，在所有java提交中击败了64.71%的用户。

## 官方解题

&emsp;官方解法更繁琐。