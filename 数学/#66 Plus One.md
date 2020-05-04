[toc]

Given a **non-empty** array of digits representing a non-negative integer, plus one to the integer.

The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.

You may assume the integer does not contain any leading zero, except the number $0$ itself.



## 题目解读

&emsp;数组形式的数字加一。

```java
class Solution {
    public int[] plusOne(int[] digits) {

    }
}
```

## 程序设计

* 需注意进位导致数组需要加长的情况。

```java
class Solution {
    public int[] plusOne(int[] digits) {
        if (digits == null || digits.length == 0) return new int[]{1};
        
        int m = digits.length, idx = m - 1;
        // 遍历进位，并将9变为0
        while (idx >= 0 && digits[idx] == 9) {
            digits[idx--] = 0;
        }
        // 数组加长
        if (idx < 0) {
            int[] temp = new int[m + 1];
            temp[0] = 1;
            digits = temp;
        } 
        // 加一
        else digits[idx]++;
        return digits;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.9MB，在所有java提交中击败了5.63%的用户。

## 官方解题

&emsp;暂无，密切关注。