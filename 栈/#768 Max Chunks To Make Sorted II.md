[toc]

This question is the same as "Max Chunks to Make Sorted" except the integers of the given array are not necessarily distinct, the input array could be up to length `2000`, and the elements could be up to `10**8`.

Given an array arr of integers (**not necessarily distinct**), we split the array into some number of "chunks" (partitions), and individually sort each chunk.  After concatenating them, the result equals the sorted array.

What is the most number of chunks we could have made?



**Note**:

* `arr` will have length in range `[1, 2000]`.
* `arr[i]` will be an integer in range `[0, 10**8]`.



## 题目解读

&emsp;不同于[#769 Max Chunks To Make Sorted](../其他/#769 Max Chunks To Make Sorted.md)，数字不再连续有限制，而且可以重复，求最多的分块。

```java
class Solution {
    public int maxChunksToSorted(int[] arr) {

    }
}
```

## 程序设计

* 首先拷贝原数组并排序，然后遍历比较前缀和，如果前缀和相等，则说明当前位置未排序的数组已包含排序后的所有元素，可以分为一块。

```java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        if (arr == null || arr.length == 0) return 0;

        int[] temp = arr.clone();
        Arrays.sort(temp);
        int count = 0, sortSum = 0, numSum = 0;
        for (int i = 0; i < arr.length; i++) {
            sortSum += temp[i];
            numSum += arr[i];
            if (sortSum == numSum) count++;
        }

        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了61.39%的用户。

内存消耗：39.9MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;官方解题思路大致相似。社区采用单调栈的方法解题。栈中记录每个分块的最大值，如果新的数据大于等于栈顶最大值，则可以作为一个新的分块，加入栈顶；如果当前数字比栈顶值小，则说明需要合并分块，记录栈顶最大值，将栈中大于当前数字的块出栈合并，最后入栈块的最大值。

```java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        if (arr == null || arr.length == 0) return 0;

        int peek = -1;
        int[] stack = new int[arr.length];
        for (int num : arr) {
            // 比栈顶值大，加入暂时作为一个块
            if (peek < 0 || stack[peek] <= num) stack[++peek] = num;
            // 比栈顶值小，需要合并到前面的块中
            else {
                // 记录合并块的最大值（为当前栈顶值）
                int max = stack[peek--];
                // 将最大值大于当前数的块出栈合并
                while (peek >= 0 && stack[peek] > num) peek--;
                // 放入合并后块的最大值
                stack[++peek] = max;
            }
        }

        // 返回栈的容量
        return peek + 1;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了99.37%的用户。

内存消耗：39.5MB，在所有java提交中击败了100.00%的用户。