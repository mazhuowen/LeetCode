[toc]

Given a **non-decreasing** array of positive integers `nums` and an integer `K`, find out if this array can be divided into one or more **disjoint increasing subsequences of length at least** `K`.



**Note:**

* $1 \le \text{nums.length} \le 10^5$
* $1 \le K \le \text{nums.length}$
* $1 \le \text{nums[i]} \le 10^5$



## 题目解读

&emsp;判断是否可以将非递减数组划分为长度至少为$K$的不相交递增数组。

```java
class Solution {
    public boolean canDivideIntoSubsequences(int[] nums, int K) {

    }
}
```

## 程序设计

* 由于需要切分为严格递增序列，故最少的切割数取决于数组中相同数字的最大数目，设为$M$，则划分数组数目最少为$M$，其他数字均可平摊到这$M$个数组上，问题转化为$M * K$是否大于等于数组长度，否则平摊后凑不够$K$个。

```java
class Solution {
    public boolean canDivideIntoSubsequences(int[] nums, int K) {
        if (K == 1) return true;
        int pre = nums[0];
        int count = 0;

        for (int num : nums) {
            if (num == pre) count++;
            else {
                pre = num;
                count = 1;
            }

            if (count * K > nums.length) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：54.3MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。