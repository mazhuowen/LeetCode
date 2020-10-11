[toc]

Given an array of numbers `nums`, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.



Note:

* The order of the result is not important. So in the above example, `[5, 3]` is also correct.
* Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?



## 题目解读

&emsp;给定数组，除了两个数是单独出现，其他数字是成对出现。求这两个数。

```java
class Solution {
    public int[] singleNumber(int[] nums) {

    }
}
```

## 程序设计

* 首先亦或得到这两个数的亦或值，取第一个1，即这两个数不相同的位数，将数组拆分为两半，然后亦或得到答案。

```java
class Solution {
    public int[] singleNumber(int[] nums) {
        if (nums == null || nums.length < 2) throw new IllegalArgumentException("invalid param");

        // xor为两个数的亦或值
        int xor = 0;
        for (int num : nums) xor ^= num;

        int flag = getFlag(xor);
        int num1 = 0, num2 = 0;
        for (int num : nums) {
            if ((num & flag) == flag) num1 ^= num;
            else num2 ^= num;
        }
        return new int[]{num1, num2};
    }

    // 获取亦或值的第一个1
    private int getFlag(int xor) {
        int k = Integer.numberOfTrailingZeros(xor);
        return 1 << k;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：40.1MB，在所有java提交中击败了16.67%的用户。

## 官方解题

&emsp;官方思路类似，在得到区分数字时，官方使用了更巧妙的方法，即使用`x&(-x)`得到最右第一个$1$的数字。负数为掩码，即`-x = ~x + 1`。