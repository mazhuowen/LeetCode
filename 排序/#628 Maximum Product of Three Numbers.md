[toc]

Given an integer array, find three numbers whose product is maximum and output the maximum product.



Note:

* The length of the given array will be in range $[3,10^4]$ and all elements are in the range $[-1000, 1000]$.
* Multiplication of any three numbers in the input won't exceed the range of 32-bit signed integer.



## 题目解读

&emsp;给定数组，求解三个元素的最大乘积。

```java
class Solution {
    public int maximumProduct(int[] nums) {

    }
}
```

## 程序设计

* 排序，排序后最大值要么是两个最小负数和最大正数的乘积，要么是三个最大正数的乘积。

```java
class Solution {
    public int maximumProduct(int[] nums) {
        if (nums == null || nums.length < 3) throw new IllegalArgumentException("invalid param");

        Arrays.sort(nums);
        int res = Math.max(nums[0] * nums[1] * nums[nums.length - 1],
                nums[nums.length - 3] * nums[nums.length - 2] * nums[nums.length - 1]);
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：12ms，在所有java提交中击败了70.30%的用户。

内存消耗：42MB，在所有java提交中击败了7.69%的用户。

## 官方解题

&emsp;官方使用线性扫描来获得最小的两个值和最大的三个值。

```java
public class Solution {
    public int maximumProduct(int[] nums) {
        int min1 = Integer.MAX_VALUE, min2 = Integer.MAX_VALUE;
        int max1 = Integer.MIN_VALUE, max2 = Integer.MIN_VALUE, max3 = Integer.MIN_VALUE;
        for (int n: nums) {
            if (n <= min1) {
                min2 = min1;
                min1 = n;
            } else if (n <= min2) {     // n lies between min1 and min2
                min2 = n;
            }
            if (n >= max1) {            // n is greater than max1, max2 and max3
                max3 = max2;
                max2 = max1;
                max1 = n;
            } else if (n >= max2) {     // n lies betweeen max1 and max2
                max3 = max2;
                max2 = n;
            } else if (n >= max3) {     // n lies betwen max2 and max3
                max3 = n;
            }
        }
        return Math.max(min1 * min2 * max1, max1 * max2 * max3);
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了99.87%的用户。

内存消耗：41.7MB，在所有java提交中击败了7.69%的用户。