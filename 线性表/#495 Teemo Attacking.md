[toc]

In LOL world, there is a hero called Teemo and his attacking can make his enemy Ashe be in poisoned condition. Now, given the Teemo's attacking **ascending** time series towards Ashe and the poisoning time duration per Teemo's attacking, you need to output the total time that Ashe is in poisoned condition.

You may assume that Teemo attacks at the very beginning of a specific time point, and makes Ashe be in poisoned condition immediately.



**Note**:

* You may assume the length of given time series array won't exceed $10000$.
* You may assume the numbers in the Teemo's attacking time series and his poisoning time duration per attacking are non-negative integers, which won't exceed $10000000$.



## 题目解读

&emsp;给定效果释放时间和持续时间，求处于该效果的总的时间。

```java
class Solution {
    public int findPoisonedDuration(int[] timeSeries, int duration) {

    }
}
```

## 程序设计

* 类似于区间扩展，比较结束区间并扩展。

```java
class Solution {
    public int findPoisonedDuration(int[] timeSeries, int duration) {
        if (timeSeries == null || timeSeries.length == 0) return 0;
        int res = 0;
        // 起始时间及结束时间
        int start = timeSeries[0], end = start + duration;
        for (int i = 1; i < timeSeries.length; i++) {
            // 效果持续
            if (timeSeries[i] <= end) end = Math.max(end, timeSeries[i] + duration);
            // 效果结束
            else {
                res += end - start;
                start = timeSeries[i];
                end = start + duration;
            }
        }
        res += end - start;
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了98.95%的用户。

内存消耗：41.6MB，在所有java提交中击败了34.11%的用户。

## 官方解题

&emsp;官方思路类似。