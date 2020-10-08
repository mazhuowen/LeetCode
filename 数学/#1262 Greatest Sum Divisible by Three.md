[toc]

Given an array `nums` of integers, we need to find the maximum possible sum of elements of the array such that it is divisible by three.



**Constraints:**

- $1 \le \text{nums.length} \le 4 * 10^4$
- $1 \le \text{nums[i]} \le 10^4$



## 题目解读

&emsp;求数组子元素最大的和，能被三整除。

```java
class Solution {
    public int maxSumDivThree(int[] nums) {

    }
}
```

## 程序设计

* 由于整除$3$只能余$0$、$1$、$2$，可以求出数组和，如果不能被$3$整除，则减去对应的数组中余数是该数字的最小数字，如`[3,6,5,1,8]`和为$23$，余数为$2$，减去最小的余数为$2$的数字，即$5$，得到答案$18$。
* 上述思路有个缺陷，如`[2,6,2,2,7]`和为$19$，余数为$1$，最小的余数为$1$的值为$7$，但实际上$2 + 2 = 4$是最小的余数为$1$的值，即不仅要记录最小值还要记录次小值。

```java
class Solution {
    public int maxSumDivThree(int[] nums) {
       int sum = 0;
       Integer minOne = null, secOne = null, minTwo = null, secTwo = null;
       for (int num : nums) {
           sum += num;
           if (num % 3 == 1) {
               if (minOne == null || num < minOne) {
                   secOne = minOne;
                   minOne = num;
               } else if (secOne == null || num < secOne) secOne = num;
           }
           if (num % 3 == 2) {
               if (minTwo == null || num < minTwo) {
                   secTwo = minTwo;
                   minTwo = num;
               } else if (secTwo == null || num < secTwo) secTwo = num;
           }
       }

       if (sum % 3 == 0) return sum;
       else if (sum % 3 == 1) {
           if (minTwo != null && secTwo != null) minOne = Math.min(minOne, minTwo + secTwo);
           return sum - minOne;
       } else {
           if (minOne != null && secOne != null) minTwo = Math.min(minTwo, minOne + secOne);
           return sum - minTwo;
       }
    }
}
```

* 还可用动态规划，使用三个变量记录余数为$0$、$1$、$2$的最大和。

```java
class Solution {
    public int maxSumDivThree(int[] nums) {
        // 余数为0，1，2的最大数组和
        int[] dp = new int[]{0, -1, -1};
        for (int num : nums) {
            int[] tmp = Arrays.copyOf(dp, 3);
            for (int i = 0; i < 3; i++) {
                if (dp[i] != -1) tmp[(num + i) % 3] = Math.max(dp[(num + i) % 3], dp[i] + num);
            }
            dp = tmp;
        }
        return dp[0];
    }
}
```

## 性能分析

&emsp;数学方法时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：7ms，在所有java提交中击败了87.34%的用户。

内存消耗：45MB，在所有java提交中击败了5.18%的用户。

&emsp;动态规划时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：7ms，在所有java提交中击败了87.34%的用户。

内存消耗：42.9MB，在所有java提交中击败了49.10%的用户。

## 官方解题

&emsp;暂无，密切关注。