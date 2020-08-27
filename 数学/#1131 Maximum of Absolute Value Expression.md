[toc]

Given two arrays of integers with equal lengths, return the maximum value of:

`|arr1[i] - arr1[j]| + |arr2[i] - arr2[j]| + |i - j|`

where the maximum is taken over all $0 \le i, j < \text{arr1.length}$.



**Constraints**:

* $2 \le \text{arr1.length} == \text{arr2.length} \le 40000$
* $-10^6 \le \text{arr1[i], arr2[i]} \le 10^6$



## 题目解读

&emsp;给定两个数组，求绝对值之和的最大值。

```java
class Solution {
    public int maxAbsValExpr(int[] arr1, int[] arr2) {
        
    }
}
```

## 程序设计

* 展开绝对式，得：

$$
arr1[i] - arr1[j] + arr2[i] - arr2[j] + i - j\\
arr1[j] - arr1[i] + arr2[i] - arr2[j] + i - j\\
arr1[i] - arr1[j] + arr2[j] - arr2[i] + i - j\\
arr1[j] - arr1[i] + arr2[j] - arr2[i] + i - j\\
arr1[i] - arr1[j] + arr2[i] - arr2[j] + j - i\\
arr1[j] - arr1[i] + arr2[i] - arr2[j] + j - i\\
arr1[i] - arr1[j] + arr2[j] - arr2[i] + j - i\\
arr1[j] - arr1[i] + arr2[j] - arr2[i] + j - i
$$

* 归纳得：

$$
arr1[i] + arr2[i] + i\\
arr1[i] - arr2[i] + i\\
arr1[i] + arr2[i] - i\\
arr1[i] - arr2[i] - i
$$

即只需统计这四组得最大值与最小值。

```java
class Solution {
    public int maxAbsValExpr(int[] arr1, int[] arr2) {
        int[][] delta = new int[][]{
            {1, 1, 1},
            {1, 1, -1},
            {1, -1, 1},
            {1, -1, -1}
        };

        int[] max = new int[4];
        int[] min = new int[4];
        Arrays.fill(min, Integer.MAX_VALUE);
        Arrays.fill(max, Integer.MIN_VALUE);
        for (int i = 0; i < arr1.length; i++) {
            for (int j = 0; j < 4; j++) {
                int val = arr1[i] * delta[j][0] + arr2[i] * delta[j][1] + i * delta[j][2];
                
                max[j] = Math.max(max[j], val);
                min[j] = Math.min(min[j], val);
            }
        }
        int res = 0;
        for (int i = 0; i < 4; i++) {
            res = Math.max(res, max[i] - min[i]);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了81.58%的用户。

内存消耗：47.2MB，在所有java提交中击败了90.91%的用户。

## 官方解题

&emsp;暂无，密切关注。