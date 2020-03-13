[toc]

There are **N** students in a class. Some of them are friends, while some are not. Their friendship is transitive in nature. For example, if A is a direct friend of B, and B is a direct friend of C, then A is an indirect friend of C. And we defined a friend circle is a group of students who are direct or indirect friends.

Given a $N*N$ matrix $M$ representing the friend relationship between students in the class. If `M[i][j] = 1`, then the `i`th and `j`th students are direct friends with each other, otherwise not. And you have to output the total number of friend circles among all the students.

Note:

* N is in range `[1,200]`.
* `M[i][i] = 1` for all students.
* If `M[i][j] = 1`, then `M[j][i] = 1`.



## 题目解读

&emsp;计算朋友圈的数目。

```java
class Solution {
    public int findCircleNum(int[][] M) {

    }
}
```

## 程序设计

* 简单的不相交集应用。

```java
class Solution {
    public int findCircleNum(int[][] M) {
        if(M == null || M.length == 0) {
            return 0;
        }
        int n = M.length;
        DisJoint disJoint = new DisJoint(n);
        // 由于矩阵是对称的，只需遍历上三角
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                if(M[i][j] == 1) {
                    disJoint.union(disJoint.find(i), disJoint.find(j));
                }
            }
        }
        return disJoint.size();
    }
}

class DisJoint {
    private int[] tree;
    private int size;

    DisJoint(int size) {
        this.size = size;
        this.tree = new int[size];
        // 填充-1
        Arrays.fill(tree, -1);
    }

    public void union(int root1, int root2) {
        if(tree[root1] >= 0 || tree[root2] >= 0) throw new IllegalArgumentException("root must be negative");
        if(root1 == root2) return;
        if(tree[root1] < tree[root2]) {
            tree[root1] += tree[root2];
            tree[root2] = root1;
        } else {
            tree[root2] += tree[root1];
            tree[root1] = root2;
        }
        size--;
    }

    public int find(int idx) {
        if(tree[idx] < 0) return idx;
        return tree[idx] = find(tree[idx]);
    }

    public int size() {
        return size;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了99.92%的用户。

内存消耗：42.1MB，在所有java提交中击败了74.62%的用户。

## 官方解题

&emsp;除了不相交集，官方还有图的遍历的思路。