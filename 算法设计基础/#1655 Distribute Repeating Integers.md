[toc]

You are given an array of $n$ integers, `nums`, where there are at most $50$ unique values in the array. You are also given an array of $m$ customer order quantities, `quantity`, where `quantity[i]` is the amount of integers the `ith` customer ordered. Determine if it is possible to distribute `nums` such that:

* The `ith` customer gets **exactly** `quantity[i]` integers,
* The integers the `ith` customer gets are **all equal**, and
* Every customer is satisfied.

Return `true` if it is possible to distribute `nums` according to the above conditions.

 

**Example 1**:

```
Input: nums = [1,2,3,4], quantity = [2]
Output: false
Explanation: The 0th customer cannot be given two different integers.
```

**Example 2**:

```
Input: nums = [1,2,3,3], quantity = [2]
Output: true
Explanation: The 0th customer is given [3,3]. The integers [1,2] are not used.
```

**Example 3**:

```
Input: nums = [1,1,2,2], quantity = [2,2]
Output: true
Explanation: The 0th customer is given [1,1], and the 1st customer is given [2,2].
```

**Example 4**:

```
Input: nums = [1,1,2,3], quantity = [2,2]
Output: false
Explanation: Although the 0th customer could be given [1,1], the 1st customer cannot be satisfied.
```

**Example 5**:

```
Input: nums = [1,1,1,1,1], quantity = [2,3]
Output: true
Explanation: The 0th customer is given [1,1], and the 1st customer is given [1,1,1].
```



**Constraints**:

* $n == \text{nums.length}$
* $1 \le n \le 10^5$
* $1 \le \text{nums[i]} \le 1000$
* $m == \text{quantity.length}$
* $1 \le m \le 10$
* $1 \le \text{quantity[i]} \le 10^5$
* There are at most $50$ unique values in nums.



## 题目解读

&emsp;判断是否可以给所有的用户分配所需数目的相同数字。

```java
class Solution {
    public boolean canDistribute(int[] nums, int[] quantity) {

    }
}
```

## 程序设计

* 参考社区思路，核心点在于统计数字出现的数目，因为结果与数字不管，可转化为一个只记录数字数目的数组，从而可用`dp(i,j)`表示数组前$0 \sim i$中是否可满足状态为$j$的分配。其中$j$为状态压缩数字。

```java
class Solution {
    public boolean canDistribute(int[] nums, int[] quantity) {
        // 预处理
        int[] count = count(nums);
        int[] need = need(quantity);

        int n = count.length, m = quantity.length;
        // 前0~i个序列是否满足分配状态为j的顾客
        boolean[][] dp = new boolean[n][1 << m];
        // 初始化
        for (int i = 0; i < n; i++) dp[i][0] = true;

        int totalNum = 0;
        for (int i = 0; i < n; i++) {
            totalNum += count[i];
            // 遍历分配状态
            for (int stat = 1; stat < (1 << m); stat++) {
                // 前面已满足或总数不满足
                if (i > 0 && dp[i - 1][stat] || need[stat] > totalNum) {
                    dp[i][stat] = need[stat] <= totalNum;
                    continue;
                }

                for (int sub = stat; sub > 0; sub = (sub - 1) & stat) {
                    if (i == 0 || dp[i - 1][stat - sub]) {
                        if (need[sub] <= count[i]) {
                            dp[i][stat] = true;
                            break;
                        }
                    }
                }
            }
        }
        
        return dp[n - 1][(1 << m) - 1];
    }

    // 返回统计数目
    private int[] count(int[] nums) {
        Map<Integer, Integer> counter = new HashMap<>();
        for (int num : nums) counter.put(num, counter.getOrDefault(num, 0) + 1);

        int[] res = new int[counter.size()];
        int idx = 0;
        for (int count : counter.values()) res[idx++] = count;
        return res;
    }

    private int[] need(int[] quantity) {
        int m = quantity.length;
        int[] res = new int[1 << m];
        // 计算每个状态所需数字数目
        for (int stat = 1; stat < (1 << m); stat++) {
            for (int i = 0; i < m; i++) {
                if ((stat & (1 << i)) != 0) {
                    res[stat] = res[stat - (1 << i)] + quantity[i];
                    break;
                }
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N3^M)$，空间复杂度为$O(N2^M)$。

执行用时：57 ms, 在所有 Java 提交中击败了76.26%的用户。

内存消耗：46.6 MB, 在所有 Java 提交中击败了95.74%的用户。

## 官方解题

&emsp;暂无，密切关注。
