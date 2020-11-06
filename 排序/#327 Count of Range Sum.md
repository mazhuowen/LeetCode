[toc]

Given an integer array `nums`, return the number of range sums that lie in `[lower, upper]` inclusive.
Range sum `S(i, j)` is defined as the sum of the elements in `nums` between indices $i$ and $j$ ($i \le j$), inclusive.



**Note:**
A naive algorithm of $O(n^2)$ is trivial. You MUST do better than that.



## 题目解读

&emsp;给定数组，返回区间和在给定范围内的数目。

```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        
    }
}
```

## 程序设计

* 最基本的思路计算前缀和，然后两层循环遍历计算。

```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        if(nums.length == 0) {
            return 0;
        }
        // 注意溢出，采用长整形
        long[] preSum = new long[nums.length + 1];
        for(int i = 0; i < nums.length; i++) {
            preSum[i + 1] = nums[i] + preSum[i];
        }
        int count = 0;
        for(int i = 1; i <= nums.length; i++) {
            for(int j = 0; j < i; j++) {
                // 采用长整形
                long sum = preSum[i] - preSum[j];
                if(sum >= lower && sum <= upper) {
                    count++;
                }
            }
        }
        return count;
    }
}
```

* 根据暴力法的思路，得到前缀和后不两次循环扫描，而是分治计算。但分治计算仍然需要遍历平方次。可以使用归并排序，这样左右区间是有序的，可以使用线性扫描来计算满足条件的区间值。
* 归并阶段左右区间有序，左、右区间已统计出满足条件的区间数，现在需要统计当前区间满足条件的数目，也就是左区间跨到右区间满足条件的数目。可以利用区间的有序性，选取左区间索引`i`，右区间需要找到满足条件的前缀和区间`l`和`h`，此时数目就是`h - l`；接着遍历下一个索引`i + 1`，由于序列是有序的，左序列和增大则总的区间和减少，`l`和`h`可以继续从当前位置遍历，而不需要回到起始位置，这样整个扫描是线性的。

```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        // 计算前缀和（注意溢出）
        long[] preSum = new long[nums.length + 1];
        for (int i = 0; i < nums.length; i++) {
            preSum[i + 1] = preSum[i] + nums[i];
        }
        // 对前缀和归并并计算区间距离（注意前缀和不包含0）
        int count = mergeCount(preSum, lower, upper, 1, nums.length);
        return count;
    }

    private int mergeCount(long[] nums, int lower, int upper, int start, int end) {
        if (start > end) {
            return 0;
        } else if (start == end) {
            // 由于排序的前缀和不包含0，无法通过减0得到整段序列的和，需要对每个值进行判断
            return nums[start] >= lower && nums[end] <= upper ? 1 : 0;
        }
        int mid =  start + (end - start) / 2;
        // 左右区间满足条件的值
        int count = mergeCount(nums, lower, upper, start, mid) + mergeCount(nums, lower, upper, mid + 1, end);
        int l = mid + 1;
        int h = mid + 1;
        // 遍历当前区间（由于左右区间是有序的，扫描为线性）
        for (int i = start; i <= mid; i++) {
            while (l <= end && nums[l] - nums[i] < lower) {
                l++;
            }
            while (h <= end && nums[h] - nums[i] <= upper) {
                h++;
            }
            // i对应的值满足条件的区间数
            count += h - l;
        }
        mergeSort(nums, start, mid, end);
        return count;
    }
    // 归并排序流程
    private void mergeSort(long[] nums, int start, int mid, int end) {
        long[] temp = new long[end - start + 1];
        int i = start, j = mid + 1, idx = 0;
        // 归并
        while (i <= mid && j <= end) {
            if (nums[i] <= nums[j]) {
                temp[idx++] = nums[i++];
            } else {
                temp[idx++] = nums[j++];
            }
        }
        while (i <= mid) {
            temp[idx++] = nums[i++];
        }
        while (j <= end) {
            temp[idx++] = nums[j++];
        }
        // 复制回原数组
        for (int k = 0; k < temp.length; k++) {
            nums[start + k] = temp[k];
        }
    }
}
```

## 性能分析

&emsp;暴力法时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：190ms，在所有java提交中击败了8.48%的用户。

内存消耗：41MB，在所有java提交中击败了5.95%的用户。

&emsp;归并法时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：8ms, 在所有java提交中击败了95.96%的用户。

内存消耗：42.4MB，在所有java提交中击败了5.48%的用户。

## 官方解题

&emsp;暂无，密切关注。