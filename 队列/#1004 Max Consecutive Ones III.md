[toc]

Given an array `A` of 0s and 1s, we may change up to `K` values from `0` to `1`.

Return the length of the longest (contiguous) subarray that contains only `1s`. 



**Note:**

* $1 \le \text{A.length} \le 20000$
* $0 \le K \le \text{A.length}$
* `A[i]` is `0` or `1` 



## 题目解读

&emsp;给定包含`1`和`0`的数组，可填充`K`个数字为`1`，求最长连续为`1`的子数组长度。

```java
class Solution {
    public int longestOnes(int[] A, int K) {

    }
}
```

## 程序设计

* `K`个可填充意味这队列中最多有`K`个`0`，求符合条件的最长队列。

```java
class Solution {
    public int longestOnes(int[] A, int K) {
        if (A == null || A.length == 0) return 0;
        if (K >= A.length) return A.length;
        // 定位队列
        int first = 0, second = 0;
        while (K > 0 && second < A.length) {
            if (A[second] == 0) K--;
            second++;
        }
        // 队列遍历出队入队
        int maxLen = second - first;
        while (second < A.length) {
            // 当前值是0，出队最前面的0，加入当前值
            if (A[second++] == 0) {
                // 出队前面的1
                while (A[first] == 1) first++;
                // 出队第一个0
                first++;
            }
            // 更新长度
            maxLen = Math.max(maxLen, second - first);
        }
        return maxLen;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：3ms，在所有java提交中击败了95.99%的用户。

内存消耗：41.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。