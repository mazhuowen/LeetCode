[toc]

Assume you have an array of length $n$ initialized with all $0$'s and are given $k$ update operations.

Each operation is represented as a triplet: `[startIndex, endIndex, inc]` which increments each element of subarray `A[startIndex ... endIndex]` (startIndex and endIndex inclusive) with `inc`.

Return the modified array after all $k$ operations were executed.



**Example**:

```
Input: length = 5, updates = [[1,3,2],[2,4,3],[0,2,-2]]
Output: [-2,0,3,5,3]
Explanation:

Initial state:
[0,0,0,0,0]

After applying operation [1,3,2]:
[0,2,2,2,0]

After applying operation [2,4,3]:
[0,2,5,5,3]

After applying operation [0,2,-2]:
[-2,0,3,5,3]
```



## 题目解读

&emsp;区间加法。

```java
class Solution {
    public int[] getModifiedArray(int length, int[][] updates) {

    }
}
```

## 程序设计

* 使用差分数组。

```java
class Solution {
    public int[] getModifiedArray(int length, int[][] updates) {
        int[] res = new int[length];
        for (int[] update : updates) {
            res[update[0]] += update[2];
            if (update[1] + 1 < length) res[update[1] + 1] -= update[2];
        }
        for (int i = 1; i < length; i++) {
            res[i] += res[i - 1];
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N + K)$，空间复杂度为$O(N)$。

执行用时：2 ms, 在所有 Java 提交中击败了96.58%的用户

内存消耗：44.9 MB, 在所有 Java 提交中击败了88.50%的用户。

## 官方解题

&emsp;官方思路同上。