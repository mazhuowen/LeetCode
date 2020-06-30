[toc]

LeetCode wants to give one of its best employees the option to travel among **N** cities to collect algorithm problems. But all work and no play makes Jack a dull boy, you could take vacations in some particular cities and weeks. Your job is to schedule the traveling to maximize the number of vacation days you could take, but there are certain rules and restrictions you need to follow.

**Rules and restrictions**:

* You can only travel among **N** cities, represented by indexes from $0$ to $N-1$. Initially, you are in the city indexed $0$ on **Monday**.
* The cities are connected by flights. The flights are represented as a $N \times N$ matrix (not necessary symmetrical), called **flights** representing the airline status from the city `i` to the city `j`. If there is no flight from the city `i` to the city `j`, `flights[i][j] = 0`; Otherwise, `flights[i][j] = 1`. Also, `flights[i][i] = 0` for all i.
* You totally have **K** weeks (**each week has 7 days**) to travel. You can only take flights at most once **per day** and can only take flights on each week's **Monday** morning. Since flight time is so short, we don't consider the impact of flight time.
* For each city, you can only have restricted vacation days in different weeks, given an $N \times K$ matrix called **days** representing this relationship. For the value of `days[i][j]`, it represents the maximum days you could take vacation in the city `i` in the week `j`.

You're given the **flights** matrix and **days** matrix, and you need to output the maximum vacation days you could take during **K** weeks.


Note:

* **N** and **K** are positive integers, which are in the range of `[1, 100]`.
* In the matrix **flights**, all the values are integers in the range of `[0, 1]`.
* In the matrix **days**, all the values are integers in the range `[0, 7]`.
* You could stay at a city beyond the number of vacation days, but you should **work** on the extra days, which won't be counted as vacation days.
* If you fly from the city A to the city B and take the vacation on that day, the deduction towards vacation days will count towards the vacation days of city B in that week.
* We don't consider the impact of flight hours towards the calculation of vacation days.



## 题目解读

&emsp;力扣想让一个最优秀的员工在`N`个城市间旅行来收集算法问题。有如下限制：只能在`N`个城市之间旅行，起始为索引为0的城市星期一。这些城市通过航班相连。总共有`K`周的时间旅行。每天最多只能乘坐一次航班，并且只能在每周的星期一上午乘坐航班。对于每个城市，不同的星期休假天数是不同的，要求输出`K`周内可以休假的最长天数。

```java
class Solution {
    public int maxVacationDays(int[][] flights, int[][] days) {

    }
}
```

## 程序设计

* 首先想到的是回溯暴力尝试，需注意每次可以选择飞往其他城市，也可以选择待在当前城市。时间复杂度为$O(N^K)$，会超时。

```java
class Solution {
    int n, k;
    int maxVacation;

    public int maxVacationDays(int[][] flights, int[][] days) {
        this.n = flights.length;
        this.k = days[0].length;

        // 在起始地停留
        vacationDays(0, 0, 0, flights, days);
        // 立即飞往下一个城市
        for (int city = 0; city < n; city++) {
            if (flights[0][city] == 0) continue;
            vacationDays(city, 0, 0, flights, days);
        }
        return maxVacation;
    }

    private void vacationDays(int start, int week, int vacation, int[][] flights, int[][] days) {
        if (week == k) {
            maxVacation = Math.max(maxVacation, vacation);
            return;
        }
        vacation += days[start][week];
        // 在该城市继续停留
        vacationDays(start, week + 1, vacation, flights, days);
        // 周一飞往下一个城市
        for (int city = 0; city < n; city++) {
            if (flights[start][city] == 0) continue;
            vacationDays(city, week + 1, vacation, flights, days);
        }
    }
}
```

* 注意到休假天数与所在城市和时间息息相关，定义动态规划数组`dp(i,j)`表示第`i`周在`j`城市的累计最大休假天数。

```java
class Solution {

    public int maxVacationDays(int[][] flights, int[][] days) {
        int n = flights.length, k = days[0].length;

        int[][] dp = new int[k + 1][n];
        for (int[] array : dp) Arrays.fill(array, Integer.MIN_VALUE);
        // 起点初始化
        dp[0][0] = 0;
        
        for (int i = 0; i < k; i++) {
            for (int j = 0; j < n; j++) {
                // 当前城市不可达，跳过
                if (dp[i][j] == Integer.MIN_VALUE) continue;
                // 继续待在该城市
                dp[i + 1][j] = Math.max(dp[i + 1][j], dp[i][j] + days[j][i]);
                // 从j飞到l
                for (int l = 0; l < n; l++) {
                    if (flights[j][l] == 0) continue;
                    dp[i + 1][l] = Math.max(dp[i + 1][l], dp[i][j] + days[l][i]);
                }
            }
        }

        int res = 0;
        for (int val : dp[k]) res = Math.max(res, val);
        return res;
    }
}
```

* 压缩空间得：

```java
class Solution {

    public int maxVacationDays(int[][] flights, int[][] days) {
        int n = flights.length, k = days[0].length;

        int[] pre = new int[n];
        Arrays.fill(pre, Integer.MIN_VALUE);
        pre[0] = 0;
        for (int i = 0; i < k; i++) {
            int[] cur = new int[n];
            Arrays.fill(cur, Integer.MIN_VALUE);

            for (int j = 0; j < n; j++) {
                if (pre[j] == Integer.MIN_VALUE) continue;
                // 继续待在该城市
                cur[j] = Math.max(cur[j], pre[j] + days[j][i]);
                // 从j飞到l
                for (int l = 0; l < n; l++) {
                    if (flights[j][l] == 0) continue;
                    cur[l] = Math.max(cur[l], pre[j] + days[l][i]);
                }
            }
            pre = cur;
        }

        int res = 0;
        for (int val : pre) res = Math.max(res, val);
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2K)$，空间复杂度为$O(NK)$。

执行用时：53ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.1MB，在所有java提交中击败了100.00%的用户。

&emsp;空间优化后时间复杂度为$O(N^2K)$，空间复杂度为$O(N)$。

执行用时：63ms，在所有java提交中击败了84.31%的用户。

内存消耗：42.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供回溯算法形式更简洁：

```java
public class Solution {
    public int maxVacationDays(int[][] flights, int[][] days) {
        return dfs(flights, days, 0, 0);
    }
    public int dfs(int[][] flights, int[][] days, int cur_city, int weekno) {
        if (weekno == days[0].length) return 0;
        int maxvac = 0;
        for (int i = 0; i < flights.length; i++) {
            // 起飞或停留
            if (flights[cur_city][i] == 1 || i == cur_city) {
                int vac = days[i][weekno] + dfs(flights, days, i, weekno + 1);
                maxvac = Math.max(maxvac, vac);
            }
        }
        return maxvac;
    }
}
```

&emsp;官方动态规划从后往前迭代：

```java
public class Solution {
    public int maxVacationDays(int[][] flights, int[][] days) {
        if (days.length == 0 || flights.length == 0) return 0;
        int[][] dp = new int[days.length][days[0].length + 1];
        for (int week = days[0].length - 1; week >= 0; week--) {
            for (int cur_city = 0; cur_city < days.length; cur_city++) {
                // 待在当前城市
                dp[cur_city][week] = days[cur_city][week] + dp[cur_city][week + 1];
                // 飞往下一个城市
                for (int dest_city = 0; dest_city < days.length; dest_city++) {
                    if (flights[cur_city][dest_city] == 1) {
                        dp[cur_city][week] = Math.max(days[dest_city][week] + dp[dest_city][week + 1], dp[cur_city][week]);
                    }
                }
            }
        }
        return dp[0][0];
    }
}
```