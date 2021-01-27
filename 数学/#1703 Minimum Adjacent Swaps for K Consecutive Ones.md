[toc]

You are given an integer array, `nums`, and an integer $k$. `nums` comprises of only $0$'s and $1$'s. In one move, you can choose two **adjacent** indices and swap their values.

Return the **minimum** number of moves required so that `nums` has $k$ **consecutive** $1$'s.

 

**Example 1**:

```
Input: nums = [1,0,0,1,0,1], k = 2
Output: 1
Explanation: In 1 move, nums could be [1,0,0,0,1,1] and have 2 consecutive 1's.
```

**Example 2**:

```


Input: nums = [1,0,0,0,0,0,1,1], k = 3
Output: 5
Explanation: In 5 moves, the leftmost 1 can be shifted right until nums = [0,0,0,0,0,1,1,1].
```

**Example 3**:

```
Input: nums = [1,1,0,1], k = 2
Output: 0
Explanation: nums already has 2 consecutive 1's.
```



**Constraints**:

* $1 \le \text{nums.length} \le 10^5$
* `nums[i]` is $0$ or $1$.
* $1 \le k \le \text{sum(nums)}$



## 题目解读

&emsp;给定只包含$0$和$1$的数组，求使得$k$个$1$连续的最小移动数目。

```java
class Solution {
    public int minMoves(int[] nums, int k) {

    }
}
```

## 程序设计

* 最基本的思路是使用滑动窗口确定包含$k$个$1$的窗口，然后计算次窗口移动的最小数目，最后综合得到结果；
* 对于固定窗口的移动数目，采用动态规划`left(i)`表示$0 \sim i$区间的$1$移动到以$i$结尾的连续区域花费的代价；`right(i)`表示$i \sim n - 1$区间的$1$移动到以$i$起始的连续区域花费的代价；最后遍历求`left(i)+right(i+1)`的最小代价；
* 该方法时间复杂度为$O(N^2)$，会超时。

```java
class Solution {
    public int minMoves(int[] nums, int k) {
        if (nums == null || nums.length == 0) return 0;

        int res = Integer.MAX_VALUE;
        // 滑动窗口
        int left = 0, right = 0, count = 0;
        while (right < nums.length) {
            count += nums[right];
            while (count > k || nums[left] == 0) count -= nums[left++];
            // 当滑动窗口有k个1时，且0的数目小于res，则判断
            if (nums[right] == 1 && count == k && right - left + 1 - k < res) res = Math.min(res, moves(nums, left, right));
            // 提前结束
            if (res == 0) return res;
            right++;
        }
        return res == Integer.MAX_VALUE ? -1 : res;
    }

    private int moves(int[] nums, int left, int right) {
        // 只有一个的情况，无需移动
        if (left == right) return 0;

        // 将左侧的1移动到结尾为i的连续序列，将右侧的1移动到起始为i的连续序列所花费的代价
        int[] leftMove = new int[right - left + 1], rightMove = new int[right - left + 1];
        int leftCount = nums[left], rightCount = nums[right];
        for (int i = 1; i <= right - left; i++) {
            // 当前为1,则不移动
            if (nums[left + i] == 1) {
                leftMove[i] = leftMove[i - 1];
                leftCount++;
            } 
            // 两两交换移动左侧1的数目个
            else {
                leftMove[i] = leftMove[i - 1] + leftCount;
            }

            // 右侧同理
            if (nums[right - i] == 1) {
                rightMove[right - left - i] = rightMove[right - left - i + 1];
                rightCount++;
            } else {
                rightMove[right - left - i] = rightMove[right - left - i + 1] + rightCount;
            }
        }
        // 遍历查找代价最小的移动位置
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < right - left; i++) {
            min = Math.min(min, leftMove[i] + rightMove[i + 1]);
        }

        return min;
    }
}
```

* 参考社区思路，假设$k$个$1$的坐标为$q_0,q_1,\dots,q_{k-1}$，而移动的目标起始坐标为$p$，则移动到$p+0,p+1,\dots,p+k-1$，此时移动花费的代价为$\sum_{i=0}^{k-1}\lvert q_i - p - i\rvert$；又由于两数之差绝对值和最大，当且仅当$p$为$q_i-i$的中位数（奇数长度）或构成中位数的两个数之间任意数值（偶数长度）；
* 这样可以提前维护所有的$q_i-i$到$idx$数组，然后以窗口为$k$的大小滑动，此时我们选择中位数的第一个点位置作为起始位置$p$所在的索引，即$idx_p=\frac{left + right}{2}$，其中$right = left + k - 1$；从而代价为$\sum_{i=left}^{idx_p}idx[idx_p] - idx[i] + \sum_{i = idx_p + 1}^{right}idx[i] - idx[idx_p]$，整合得$(2idx_p - left - right + 1) * idx[idx_p] + \sum_{i=idx_p}^{right}idx[i] - \sum_{i=left}^{idx_p}idx[i]$；
* 使用前缀和来避免求和运算时的遍历。

```java
class Solution {
    public int minMoves(int[] nums, int k) {
        if (nums == null || nums.length == 0) return 0;

        int n = nums.length;
        // 将1的坐标维护进数组
        List<Integer> index = new ArrayList<>(), preSum = new ArrayList<>();
        preSum.add(0);
        int count = 0, sum = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] == 1) {
                index.add(i - count);
                sum += i - count++;
                preSum.add(sum);
            }
        }

        int m = index.size(), res = Integer.MAX_VALUE;
        for (int i = 0; i + k <= m; i++) {
            // 中位数（或者长度为偶数时，选择两个数中的第一个输）
            int idx = (i + i + k - 1) / 2;
            int cur = (2 * idx - 2 * i - k + 2) * index.get(idx) + preSum.get(i + k) - preSum.get(idx + 1) - preSum.get(idx + 1) + preSum.get(i);
            res = Math.min(res, cur);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：21 ms, 在所有 Java 提交中击败了24.03%的用户。

内存消耗：52.6 MB, 在所有 Java 提交中击败了20.13%的用户。

## 官方解题

&emsp;暂无，密切关注。
