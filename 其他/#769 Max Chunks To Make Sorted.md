[toc]

Given an array `arr` that is a permutation of `[0, 1, ..., arr.length - 1]`, we split the array into some number of "chunks" (partitions), and individually sort each chunk.  After concatenating them, the result equals the sorted array.

What is the most number of chunks we could have made?



**Note**:

* `arr` will have length in range `[1, 10]`.
* `arr[i]` will be a permutation of `[0, 1, ..., arr.length - 1]`.



## 题目解读

&emsp;给定数组，查找最多的分块数，是的每个块排序后连接得到的结果是有序的。

```java
class Solution {
    public int maxChunksToSorted(int[] arr) {

    }
}
```

## 程序设计

* 由于分块排序后的数组是整体有序的，这意味着分块内的数字是连续的。有了这个发现，可以使用有序数组的数字和和当前数组的数字和比较，相等则表示当前序列包含该位置有序序列所需所有数字，可以分块。

```java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        if (arr == null || arr.length == 0) return 0;

        int count = 0, idxSum = 0, numSum = 0;
        for (int i = 0; i < arr.length; i++) {
            idxSum += i;
            numSum += arr[i];
            if (numSum == idxSum) count++;
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37MB，在所有java提交中击败了33.33%的用户。

## 官方解题

&emsp;官方思路类似，只是用最大值代替了前缀和。

```java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        int ans = 0, max = 0;
        for (int i = 0; i < arr.length; ++i) {
            max = Math.max(max, arr[i]);
            if (max == i) ans++;
        }
        return ans;
    }
}
```