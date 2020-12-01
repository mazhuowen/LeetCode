[toc]

You are given an integer array `nums` of **even** length $n$ and an integer `limit`. In one move, you can replace any integer from `nums` with another integer between $1$ and `limit`, inclusive.

The array `nums` is **complementary** if for all indices $i$ (**0-indexed**), `nums[i] + nums[n - 1 - i]` equals the same number. For example, the array $[1,2,3,4]$ is complementary because for all indices $i$, `nums[i] + nums[n - 1 - i] = 5`.

Return the **minimum** number of moves required to make `nums` **complementary**.

 

**Example 1**:

```
Input: nums = [1,2,4,3], limit = 4
Output: 1
Explanation: In 1 move, you can change nums to [1,2,2,3] (underlined elements are changed).
nums[0] + nums[3] = 1 + 3 = 4.
nums[1] + nums[2] = 2 + 2 = 4.
nums[2] + nums[1] = 2 + 2 = 4.
nums[3] + nums[0] = 3 + 1 = 4.
Therefore, nums[i] + nums[n-1-i] = 4 for every i, so nums is complementary.
```

**Example 2**:

```
Input: nums = [1,2,2,1], limit = 2
Output: 2
Explanation: In 2 moves, you can change nums to [2,2,2,2]. You cannot change any number to 3 since 3 > limit.
```

**Example 3**:

```
Input: nums = [1,2,1,2], limit = 2
Output: 0
Explanation: nums is already complementary.
```



**Constraints**:

* $n == \text{nums.length}$
* $2 \le n \le 10^5$
* $1 \le \text{nums[i]} \le \text{limit} \le 10^5$
* $n$ is even.



## 题目解读

&emsp;题目定义数组互补为对任意的下标$i$于$n - i - 1$的数值对的和相等，求修改数组使得互补的最少步数。

```java
class Solution {
    public int minMoves(int[] nums, int limit) {

    }
}
```

## 程序设计

* 最基本的思路是计数数值对的和，选择计数最大的为基准值，修改其他数值对的值；这个思路有严重的缺陷就是其他数值对可能需要一步或两步才能修改为基准值，得到的步数不一定是最少的；
* 单个的判断每个数值对，假设最小、最大值为$min$、$max$，则基准值$[2,min]$区间内需要修改两次，两个数值都要替换；$[min + 1, min + max - 1]$只需修改一次即可，修改$max$；$min+max$无需修改；$[min + max + 1, max + limit]$只需修改一次，修改$min$；最后$[max + limit + 1,limit + limit]$需要修改两次；
* 如果为每个数值对遍历基准值数组，回超时；考虑到差分数组，只需在关键位置记录差分数值，最后统一相加即可；

```java
class Solution {
    public int minMoves(int[] nums, int limit) {
        int[] diff = new int[limit * 2 + 1];
        for (int i = 0; i < nums.length / 2; i++) {
            int min = Math.min(nums[i], nums[nums.length - i - 1]);
            int max = Math.max(nums[i], nums[nums.length - i - 1]);
            // [2, min]需要修改两次
            if (min == 1) diff[2]++;
            else {
                diff[2] += 2;
                // [min+1,min+max-1]只需修改一次，差分数组从前面的2中减去1
                diff[min + 1]--;
            }
            // min+max无需修改，减去前面的1
            diff[min + max]--;
            // [min+max+1,max+limit]只需修改一个
            if (min + max + 1 < diff.length) diff[min + max + 1]++;
            // [max+limit+1, limit+limit]需要修改两次，差分加一
            if (max + limit + 1 < diff.length) diff[max + limit + 1]++;
        }

        int res = nums.length / 2, sum = 0;
        for (int i = 2; i < diff.length; i++) {
            sum += diff[i];
            res = Math.min(res, sum);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N + limit)$，空间复杂度为$O(limit)$。



## 官方解题

&emsp;暂无，密切关注。