[toc]

Given a binary array `data`, return the minimum number of swaps required to group all `1`’s present in the array together in **any place** in the array.



**Note**:

* $1 \le \text{data.length} \le 10^5$
* $0 \le \text{data[i]} \le 1$



## 题目解读

&emsp;给定一个二进制数组，求使得所有$1$在一起的最少交换次数。

```java
class Solution {
    public int minSwaps(int[] data) {
        
    }
}
```

## 程序设计

* 假设$1$的数目有$m$个，则问题转化为$m$大小的窗口中最多的$1$的数目，剩余的最少的$1$只需交换到该窗口内即可。

```java
class Solution {
    public int minSwaps(int[] data) {
        if (data == null || data.length == 0) return 0;

        // 统计1的个数
        int ones = 0;
        for (int num : data) if (num == 1) ones++;
        if (ones <= 1) return 0;

        // 滑动窗口定位
        int max = 0, idx = 0, count = 0;
        while (idx < ones) {
            count += data[idx++];
        }
        // 统计窗口内最多的1
        max = count;
        while (idx < data.length) {
            count -= data[idx - ones];
            count += data[idx++];
            max = Math.max(max, count);
        }

        return ones - max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：5ms，在所有java提交中击败了84.75%的用户。

内存消耗：50.9MB，在所有java提交中击败了52.00%的用户。

## 官方解题

&emsp;同上。