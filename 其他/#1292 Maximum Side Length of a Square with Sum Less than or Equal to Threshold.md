[toc]

Given a $m \times n$ matrix `mat` and an integer `threshold`. Return the maximum side-length of a square with a sum less than or equal to `threshold` or return 0 if there is no such square.

 

Constraints:

* $1 \le m, n \le 300$
* $m == \text{mat.length}$
* $n == \text{mat[i].length}$
* $0 \le \text{mat[i][j]} \le 10000$
* $0 \le \text{threshold} \le 10^5$



## 题目解读

&emsp;给定矩形，给出面积不超过阈值的正方形的最大边长。

```java
class Solution {
    public int maxSideLength(int[][] mat, int threshold) {

    }
}
```

## 程序设计

* 最基本的思路是使用矩形前缀和遍历计算每个可能的正方形面积，并更新结果。

```java
class Solution {
    public int maxSideLength(int[][] mat, int threshold) {
        if (mat == null || mat.length == 0 || mat[0].length == 0) return 0;

        int m = mat.length, n = mat[0].length;

        // 计算矩形前缀和
        int[][] sum = new int[m][n];
        for (int i = 0; i < m; i++) {
            int[] preSum = new int[n + 1];
            for (int j = 0; j < n; j++) {
                preSum[j + 1] = preSum[j] + mat[i][j];
                sum[i][j] = preSum[j + 1] + (i == 0 ? 0 : sum[i - 1][j]);
            }
        }

        // 依次遍历比较所有面积
        int maxLen = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == j && sum[i][j] <= threshold) {
                    maxLen = Math.max(maxLen, i + 1);
                    continue;
                }
                // k为正方形边长
                for (int k = 1; k <= i && k <= j; k++) {
                    // 正方形面积
                    int area = sum[i][j] - sum[i - k][j] - sum[i][j - k] + sum[i - k][j - k];
                    if (area <= threshold) maxLen = Math.max(maxLen, k);
                }
            }
        }
        return maxLen;
    }
}
```

* 考虑到上述思路遍历正方形的对角线并验证，由于面积非负，具有有序性，可以使用二分查找代替遍历。

```java
class Solution {
    public int maxSideLength(int[][] mat, int threshold) {
        if (mat == null || mat.length == 0 || mat[0].length == 0) return 0;

        int m = mat.length, n = mat[0].length;

        // 计算矩形前缀和
        int[][] sum = new int[m][n];
        for (int i = 0; i < m; i++) {
            int[] preSum = new int[n + 1];
            for (int j = 0; j < n; j++) {
                preSum[j + 1] = preSum[j] + mat[i][j];
                sum[i][j] = preSum[j + 1] + (i == 0 ? 0 : sum[i - 1][j]);
            }
        }

        // 依次遍历比较所有面积
        int maxLen = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == j && sum[i][j] <= threshold) {
                    maxLen = Math.max(maxLen, i + 1);
                    continue;
                }
                // 二分查找代替遍历
                int left = 1, right = Math.min(i, j);
                while (left < right) {
                    int mid = left + (right - left) / 2;
                    // 正方形面积
                    int area = sum[i][j] - sum[i - mid][j] - sum[i][j - mid] + sum[i - mid][j - mid];
                    if (area > threshold) right = mid;
                    else left = mid + 1;
                }

                maxLen = Math.max(maxLen, left - 1);
            }
        }
        return maxLen;
    }
}
```

## 性能分析

&emsp;暴力法时间复杂度为$O(MN\min(M,N))$，空间复杂度为$O(MN)$。

执行用时：106ms，在所有java提交中击败了36.65%的用户。

内存消耗：49.5MB，在所有java提交中击败了100.00%的用户。

&emsp;二分法时间复杂度为$O(MN\log_2\min(M,N))$，空间复杂度为$O(MN)$。

执行用时：26ms，在所有java提交中击败了52.19%的用户。

内存消耗：49.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方思路在上述基础上继续优化，遍历查找可以在当前最大边长继续遍历，而不是从1开始遍历。优化后的遍历法：

```java
class Solution {
    public int maxSideLength(int[][] mat, int threshold) {
        if (mat == null || mat.length == 0 || mat[0].length == 0) return 0;

        int m = mat.length, n = mat[0].length;

        // 计算矩形前缀和
        int[][] sum = new int[m][n];
        for (int i = 0; i < m; i++) {
            int[] preSum = new int[n + 1];
            for (int j = 0; j < n; j++) {
                preSum[j + 1] = preSum[j] + mat[i][j];
                sum[i][j] = preSum[j + 1] + (i == 0 ? 0 : sum[i - 1][j]);
            }
        }

        // 依次遍历比较所有面积
        int maxLen = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == j && sum[i][j] <= threshold) {
                    maxLen = Math.max(maxLen, i + 1);
                    continue;
                }
                // k为正方形边长
                for (int k = maxLen + 1; k <= i && k <= j; k++) {
                    // 正方形面积
                    int area = sum[i][j] - sum[i - k][j] - sum[i][j - k] + sum[i - k][j - k];
                    // 不存在
                    if (area > threshold) break;
                    maxLen = Math.max(maxLen, k);
                }
            }
        }
        return maxLen;
    }
}
```

&emsp;优化后的算法时间复杂度是多少呢？显然等于第三重循环中边长k被枚举的次数。优化后第三重循环的上下界并不固定，将第三重循环中边长k的枚举分为两类：

* 成功枚举：如果当前枚举的边长为k的正方形的元素之和不超过阈值，那么称此为一次成功枚举。在进行成功枚举后，找到了比之前边长更大的正方形。

* 失败枚举：如果当前枚举的边长为k的正方形的元素之和大于阈值，那么称此为一次失败枚举。在进行失败枚举后，就没有必要枚举更大的边长了，直接跳出第三重循环。

对于成功枚举而言，由于每进行一次成功枚举都会得到一个边长更大的正方形，而边长的最大值不会超过$\min(m, n)$，因此成功枚举的总次数也不会超过$\min(m, n)$；对于失败枚举而言，由于每进行一次失败枚举都会直接跳出第三重循环，因此每一个左上角的位置`(i, j)`最多只会对应一次失败枚举，即失败枚举的总次数不会超过$MN$。因此优化后算法的时间复杂度为$O(\min(M, N) + MN) = O(MN)$。

执行用时：5ms，在所有java提交中击败了100.00%的用户。

内存消耗：49.9MB，在所有java提交中击败了100.00%的用户。