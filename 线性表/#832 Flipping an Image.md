[toc]

Given a binary matrix `A`, we want to flip the image horizontally, then invert it, and return the resulting image.

To flip an image horizontally means that each row of the image is reversed.  For example, flipping `[1, 1, 0]` horizontally results in `[0, 1, 1]`.

To invert an image means that each `0` is replaced by `1`, and each `1` is replaced by `0`. For example, inverting `[0, 1, 1]` results in `[1, 0, 0]`.



**Notes:**

- $1 \le \text{A.length} = \text{A[0].length} \le 20$
- $0 \le \text{A[i][j]} \le 1$



## 题目解读

&emsp;交换每行数据并取反。

```java
class Solution {
    public int[][] flipAndInvertImage(int[][] A) {

    }
}
```

## 程序设计

* 数组交换。

```java
class Solution {
    public int[][] flipAndInvertImage(int[][] A) {
        if (A == null || A.length == 0 || A[0].length == 0) return A;

        // 交换每行数据
        for (int i = 0; i < A.length; i++) {
            swapAndInvert(A[i]);
        }
        return A;
    }

    // 交换并取反
    private void swapAndInvert(int[] row) {
        int left = 0, right = row.length - 1;
        while (left < right) {
            int temp = row[left];
            row[left++] = row[right] ^ 1;
            row[right--] = temp ^ 1;
        }
        if (left == right) row[left] ^= 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M*N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.8MB，在所有java提交中击败了13.33%的用户。

## 官方解题

&emsp;官方思路类似。