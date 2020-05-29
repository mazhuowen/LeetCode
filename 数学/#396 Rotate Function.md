[toc]

Given an array of integers `A` and let `n` to be its length.

Assume `Bk` to be an array obtained by rotating the array `A` k positions clock-wise, we define a "rotation function" `F` on `A` as follow:

$$
F(k) = 0 * B_k[0] + 1 * B_k[1] + ... + (n-1) * B_k[n-1]
$$
Calculate the maximum value of `F(0)`, `F(1)`, ..., `F(n-1)`.



Note:
`n` is guaranteed to be less than $10^5$.



## 题目解读

&emsp;数组旋转函数最大值求解。旋转函数定义为数组值与索引的乘积和。

```java
class Solution {
    public int maxRotateFunction(int[] A) {

    }
}
```

## 程序设计

* 由于数组顺时针移动，除了最右端的值索引变为0，其他值索引都增加了1，这样新的旋转函数就是旧的旋转函数值加数组和，然后减去原先最右端的值乘以数组长度。

```java
class Solution {
    public int maxRotateFunction(int[] A) {
        if (A == null || A.length == 0) return 0;

        int n = A.length;
        int sum = 0, rotate = 0;
        for (int i = 0; i < n; i++) {
            sum += A[i];
            rotate += i * A[i];
        }
        int maxRotate = rotate;
        for (int i = n - 1; i >= 0; i--) {
            rotate = rotate + sum - n * A[i];
            maxRotate = Math.max(maxRotate, rotate);
        }
        return maxRotate;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.1MB，在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;暂无，密切关注。