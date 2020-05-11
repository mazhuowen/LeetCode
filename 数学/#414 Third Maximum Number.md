[toc]

Given a **non-empty** array of integers, return the **third** maximum number in this array. If it does not exist, return the maximum number. The time complexity must be in $O(n)$.



## 题目解读

&emsp;返回第三大的数据，没有则返回最大的数。

```java
class Solution {
    public int thirdMax(int[] nums) {

    }
}
```

## 程序设计

* 记录前三大的数值。注意当第三大的值恰好是最小负数的边界情况。

```java
class Solution {
    public int thirdMax(int[] nums) {
        if (nums == null || nums.length == 0) throw new IllegalArgumentException("invalid param");

        int max1 = Integer.MIN_VALUE, max2 = Integer.MIN_VALUE, max3 = Integer.MIN_VALUE;
        // 记录数组中是否存在最小负数
        boolean flag = false;
        // 遍历记录前三大的值
        for (int num : nums) {
            if (num == Integer.MIN_VALUE) flag = true;
            // 题目要求不重复
            if (num == max1 || num == max2 || num == max3) continue;

            if (num > max1) {
                max3 = max2;
                max2 = max1;
                max1 = num;
            } else if (num > max2) {
                max3 = max2;
                max2 = num;
            } else if (num > max3) {
                max3 = num;
            }
        }
        // 存在第三大的值（特别判断边界条件）
        if (max3 > Integer.MIN_VALUE || (flag && max2 > Integer.MIN_VALUE)) return max3;
        else return max1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了96.54%的用户。

内存消耗：39.5MB，在所有java提交中击败了11.11%的用户。

## 官方解题

&emsp;暂无，密切关注。