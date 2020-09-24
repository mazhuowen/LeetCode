[toc]

为了提高自己的代码能力，小张制定了 LeetCode 刷题计划，他选中了 LeetCode 题库中的 $n$ 道题，编号从 $0$ 到 $n-1$，并计划在 $m$ 天内按照题目编号顺序刷完所有的题目（注意，小张不能用多天完成同一题）。

在小张刷题计划中，小张需要用 `time[i]` 的时间完成编号 $i$ 的题目。此外，小张还可以使用场外求助功能，通过询问他的好朋友小杨题目的解法，可以省去该题的做题时间。为了防止“小张刷题计划”变成“小杨刷题计划”，小张每天最多使用一次求助。

我们定义 $m$ 天中做题时间最多的一天耗时为 $T$（小杨完成的题目不计入做题总时间）。请你帮小张求出最小的$T$是多少。



**限制：**

- $1 \le \text{time.length} \le 10^5$
- $1 \le \text{time[i]} \le 10000$
- $1 \le m \le 1000$



## 题目解读

&emsp;求每天做题的花费最多时间的最小值。

```java
class Solution {
    public int minTime(int[] time, int m) {

    }
}
```

## 程序设计

* 如果不考虑小杨代做，则问题转化为数组划分为$m$份的最小最大子数组和，使用二分查找尝试；
* 考虑小杨代做，则只需将子数组中最大的值减去即可。

```java
class Solution {
    public int minTime(int[] time, int m) {
        // 每天请小杨帮忙
        if (time.length <= m) return 0;

        int n = time.length;
        // 二分查找尝试最小划分
        int left = 0, right = 0;
        for (int t : time) right += t;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (check(mid, time, m)) right = mid;
            else left = mid + 1;
        }
        return left;
    }

    // 贪婪法划分
    private boolean check(int limit, int[] time, int m) {
        int num = 0, sum = 0, max = 0;
        for (int t : time) {
            sum += t;
            max = Math.max(max, t);
            // 减去最大值大于限制，则需要重新划分
            if (sum - max > limit) {
                num++;
                sum = max = t;
            }
        }
        // 最后一段
        num++;
        return num <= m;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2M)$，空间复杂度为$O(1)$。

执行用时：8ms，在所有java提交中击败了97.45%的用户。

内存消耗：46.6MB，在所有java提交中击败了44.50%的用户。

## 官方解题

&emsp;官方思路类似。