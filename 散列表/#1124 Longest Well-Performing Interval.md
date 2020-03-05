[toc]

We are given `hours`, a list of the number of hours worked per day for a given employee.

A day is considered to be a `tiring day` if and only if the number of hours worked is (strictly) greater than 8.

A well-performing interval is an interval of days for which the number of tiring days is strictly larger than the number of non-tiring days.

Return the length of the longest well-performing interval.



**Constraints:**

- $1 \le \text{hours.length} \le 10000$
- $0 \le \text{hours[i]} \le 16$



## 题目解读

&emsp;给定一份工作时间表,上面记录着某一位员工每天的工作小时数。当员工一天中的工作小时数大于8小时的时候，那么这一天就是劳累的一天。表现良好的时间段表示在这段时间内，劳累的天数是严格大于不劳累的天数。返回表现良好时间段的最大长度。

```java
class Solution {
    public int longestWPI(int[] hours) {

    }
}
```

## 程序设计

* 可以将大于8的记为1，小于等于8的记为-1，然后转化为前缀和。这样问题就变成了求连续子序列使得和大于0。

```java
class Solution {
    public int longestWPI(int[] hours) {
        int[] preSum = new int[hours.length + 1];
        for(int i = 0; i < hours.length; i++) {
            preSum[i + 1] = preSum[i] + (hours[i] > 8 ? 1 : -1);
        }
        int time = 0;
        for(int i = 0; i < hours.length; i++) {
            for(int j = i + 1; j <= hours.length; j++) {
                // 区间和大于0，即表示大于8小时的比少于8小时的多，更新
                if(preSum[j] - preSum[i] > 0) {
                    time = Math.max(time, j - i);
                }
            }
        }
        return time;
    }
}
```

* 注意到暴力法内层循环需要找到最前面那个小于当前前缀和的位置，如果是有序的就可以使用二分查找来优化。事实上我们只关心最前面那个小于当前前缀和的位置，其后的值是大是小没有影响，可以引入数组记录当前位置之前的最小前缀和，这样数组是有序降低的，问题转变成存在重复数字的有序数组寻找第一个数字的问题。

```java
class Solution {
    public int longestWPI(int[] hours) {
        int time = 0;
        // 前缀和
        int[] preSum = new int[hours.length + 1];
         // 当前位置及之前最小的和，使得数组成为降序数组，可以使用二分查找
        int[] minSum  = new int[hours.length + 1];
        for(int i = 0; i < hours.length; i++) {
            preSum[i + 1] = preSum[i] + (hours[i] > 8 ? 1 : -1);
            minSum[i + 1] = Math.min(minSum[i], preSum[i + 1]);
            // 查找i + 1之前小于preSum[i + 1]的最前面的值
            // 最终left为位置（如果存在）或i（如果不存在，需要额外判断）
            int left = 0, right = i;
            while(left < right) {
                int mid = left + (right - left) / 2;
                // 数组降序，向右查找
                if(minSum[mid] >= preSum[i + 1]) {
                    left = mid + 1;
                } 
                // minSum[mid] < preSum[i + 1]，right保持mid
                else {
                    right = mid;
                }
            }
            // 额外判断left
            if(minSum[left] >= preSum[i + 1]) {
                continue;
            }
            time = Math.max(time, i + 1 - left);
        }
        return time;
    }
}
```

## 性能分析

&emsp;前缀和方法时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：735ms，在所有java提交中击败了6.45%的用户。

内存消耗：42.1MB，在所有java提交中击败了5.69%的用户。

&emsp;二分查找时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：19ms，在所有java提交中击败了85.48%的用户。

内存消耗：42.3MB，在所有java提交中击败了5.69%的用户。

## 官方解题

&emsp;官方的暴力法更合理：

```java
class Solution {
    public int longestWPI(int[] hours) {
        int time = 0;
        int[] preSum = new int[hours.length + 1];
        for(int i = 0; i < hours.length; i++) {
            preSum[i + 1] = preSum[i] + (hours[i] > 8 ? 1 : -1);
            // 从前面开始遍历
            for(int j = 0; j <= i; j++) {
                if(preSum[i + 1] - preSum[j] > 0) {
                    time = Math.max(time, i + 1 - j);
                    // 由于是从前遍历，满足要求的就是最长的，结束本轮循环
                    break;
                }
            }
        }
        return time;
    }
}
```

&emsp;除了上面二分查找法，官方给出的最佳解法为哈希表法，通过发现前缀和的规律，使用哈希表存储计算。

```java
class Solution {
    public int longestWPI(int[] hours) {
        // 记录前缀和及其索引
        Map<Integer, Integer> map = new HashMap<>();
        int len = 0;
        int preSum = 0;
        for(int i = 0; i < hours.length; i++) {
            preSum += hours[i] > 8 ? 1 : -1;
            // 前缀和大于0，表示当前到开头满足条件
            if(preSum > 0) {
                len = i + 1;
            } 
            // 前缀和小于等于0，需要记录
            else {
                // 存在则不更新，保留在最前面的索引
                map.putIfAbsent(preSum, i);
                // 如果preNum-1存在，此时这两个前缀和相减大于0，符合要求，取出索引更新
                // 由于前缀和不是渐变的，要么加一要么减一是连续的。因此如果存在比当前preSum小的前缀和，必然存在preSum - 1
                // 即使存在其它更小的前缀和preSum-n，由于连续性，preSum-1必然出现在其前面，也就是说preSum-1到preSum的距离最大
                if(map.get(preSum - 1) != null) {
                    len = Math.max(len, i - map.get(preSum - 1));
                }
            }
        }
        return len;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：14ms，在所有java提交中击败了91.13%的用户。

内存消耗：42.6MB，在所有java提交中击败了5.69%的用户。

> 社区一些性能更快的方法思路一致，只是用数组代替了字典，同时为了适应这个变化，计算前缀和时大于8减一，小于等于8加一，方便放入数组的索引是正数。