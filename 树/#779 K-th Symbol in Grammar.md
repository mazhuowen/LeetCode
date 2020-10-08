[toc]

On the first row, we write a $0$. Now in every subsequent row, we look at the previous row and replace each occurrence of $0$ with $01$, and each occurrence of $1$ with $10$.

Given row $N$ and index $K$, return the $K$-th indexed symbol in row $N$. (The values of $K$ are 1-indexed.) (1 indexed).



**Note**:

* $N$ will be an integer in the range $[1, 30]$.
* $K$ will be an integer in the range $[1, 2^(N-1)]$.



## 题目解读

&emsp;给定行数，求第$k$个值。

```java
class Solution {
    public int kthGrammar(int N, int K) {

    }
}
```

## 程序设计

* 由于是二叉树的结构，递归获取当前值。

```java
class Solution {
    public int kthGrammar(int N, int K) {
        if (N <= 0) throw new IllegalArgumentException("invalid param");
        if (N == 1 && K == 1) return 0;
        int pre = kthGrammar(N - 1, (K + 1) / 2);
        // 父节点为0，左节点为0右节点为1
        if (pre == 0) return K % 2 == 1 ? 0 : 1;
        // 父节点为1，左节点为1右节点为0
        else return K % 2 == 1 ? 1 : 0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(\log_2N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：35.6MB，在所有java提交中击败了45.71%的用户。

## 官方解题

&emsp;官方还提出了其他规律，并使用位运算优化计算过程。