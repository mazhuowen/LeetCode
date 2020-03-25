[toc]

We have `n` jobs, where every job is scheduled to be done from `startTime[i]` to `endTime[i]`, obtaining a profit of `profit[i]`.

You're given the `startTime` , `endTime` and `profit` arrays, you need to output the maximum profit you can take such that there are no 2 jobs in the subset with overlapping time range.

If you choose a job that ends at time `X` you will be able to start another job that starts at time `X`.



**Constraints:**

- $1 \le \text{startTime.length} == \text{endTime.length} == \text{profit.length} \le 5 * 10^4$
- $1 \le \text{startTime[i]} < \text{endTime[i]} \le 10^9$
- $1 \le \text{profit[i]} \le 10^4$



## 题目解读

&emsp;给定工作的时间表和酬金，计算能得到的最大收入。

```java
class Solution {
    public int jobScheduling(int[] startTime, int[] endTime, int[] profit) {

    }
}
```

## 程序设计

* 首先观察示例，要计算当前区间可获得的薪酬，需要遍历之前的结束时间在当前开始时间前的的区间，取最大值和当前区间相加，就得到了包含当前区间的最大薪酬；为了方便统计计算，可以创建动态规划数组，保存截止每个区间的薪酬。
* 以开始时间`[1,2,3,4,6]`，结束时间`[3,5,10,6,9]`，酬劳`[20,20,100,70,60]`为例，区间`[1,3]`没有比它更早的区间，数组记录薪酬为`20`，区间`[2,5]`同理，记录为`20`，区间`[3,10]`之前的区间只有`[1,3]`，累计为`20 + 100 = 120`，区间`[4,6]`前的区间只有`[1,3]`，累计为`90`，区间`[6,9]`之前有`[4,6]`和`[2,5]`，选取大的一个，累计为`90 + 60 = 150`，最后可以遍历数组得到最大值。但是存在一个问题，这种思路需要遍历多次，首先查找在开始时间之前的区间，然后比对计算这些区间，不满足题目规模要求。
* 为了减少比对计算，动态规划数组可以保存当前结束时间截止可获得的最大酬劳，这样省去遍历计算的问题；但是还是需要定位到小于开始时间的结束时间位置，如果结束时间是有序的，则可以将线性遍历转化为对数搜索。
* 结合上面两个思路得到总体思想如下：对工作的结束时间排序，动态规划数组保存截止当前结束时间的最大酬劳，这样只需一次遍历即可。每次遍历到当前工作区间，首先二分定位到开始时间前的结束时间，由于记录了最大酬劳，和当前区间酬劳相加就是包含当前工作的最大酬劳；还有一种可能是不选择当前区间，此时酬劳就是前一个结束时间的酬劳，将这两中方案对比，得到截止目前结束时间的最大酬劳，更新数组。最后返回数组最后一位即可。

```java
class Solution {
    public int jobScheduling(int[] startTime, int[] endTime, int[] profit) {
        if (startTime == null || startTime.length != endTime.length || startTime.length != profit.length) throw new IllegalArgumentException("invalid param");
        int jobs = startTime.length;
        // 动态规划数组，记录截止当前结束时间最大的收入
        int[] dp = new int[jobs];
        // 索引数组
        Integer[] idx = new Integer[jobs];
        for (int i = 0; i < jobs; i++) {
            idx[i] = i;
        }
        // 根据结束时间排序
        Arrays.sort(idx, (a, b) -> endTime[a] - endTime[b]);

        for (int i = 0; i < jobs; i++) {
            int start = startTime[idx[i]];

            int left = 0, right = i;
            // 二分查找第一个大于start的结束时间（即其前面小于等于开始时间）
            while (left < right) {
                int mid = left + (right - left) / 2;
                // 结束时间大于当前开始时间，继续向前搜索，右边界保持大于开始时间的第一个结束时间
                if (endTime[idx[mid]] > start) {
                    right = mid;
                } else {
                    left = mid + 1;
                }
            }
            // left为0，表示所有结束时间都大于当前开始时间
            if (left == 0) {
                // 如果当前结束时间是最小的，则目前为止最大的收入为当前工作，否则为当前工作和之前结束时间收入的最大值
                dp[i] = i == 0 ? profit[idx[i]] : Math.max(profit[idx[i]], dp[i - 1]);
            } else {
                // 截止目前最大收入为前一时刻和当前工作加开始时间前最大收入中的最大值
                dp[i] = Math.max(dp[left - 1] + profit[idx[i]], dp[i - 1]);
            }
        }
        // 返回最大值
        return dp[jobs - 1];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：32ms，在所有java提交中击败了74.66%的用户。

内存消耗：45.3MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区思路基本一致，根据结束时间排序，然后动态规划。