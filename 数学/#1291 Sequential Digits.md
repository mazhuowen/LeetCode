[toc]

An integer has sequential digits if and only if each digit in the number is one more than the previous digit.

Return a **sorted** list of all the integers in the range `[low, high]` inclusive that have sequential digits.

 

**Example 1**:

```
Input: low = 100, high = 300
Output: [123,234]
```

**Example 2**:

```
Input: low = 1000, high = 13000
Output: [1234,2345,3456,4567,5678,6789,12345]
```



**Constraints**:

* $10 \le \text{low} \le \text{high} \le 10^9$



## 题目解读

&emsp;定义顺次数为每一位的数字都比前一位的数字大$1$，求给定区间内的顺次数。

```java
class Solution {
    public List<Integer> sequentialDigits(int low, int high) {

    }
}
```

## 程序设计

* 观察到长度相等的顺次数之间是递增的，可求得最小顺次数和递增值，进行遍历。

```java
class Solution {
    public List<Integer> sequentialDigits(int low, int high) {
        List<Integer> res = new LinkedList<>();
        if (low > high) return res;
        // 长度、当前长度最小的顺次数、当前长度的递增值
        int len = len(low), num = init(len), gain = gain(len);
        while (num <= high) {
            // 符合条件
            if (num >= low) res.add(num);
            // 当前长度顺次数已遍历完，定位到下一个长度
            if (num % 10 == 9) {
                len++;
                num = init(len);
                gain = gain(len);
            } 
            // 递增，迭代到下一个顺次数
            else {
                num += gain;
            }
        }
        return res;
    }

    private int len(int num) {
        int len = 0;
        while (num > 0) {
            num /= 10;
            len++;
        }
        return len;
    }

    // 当前长度的最小顺次数
    private int init(int len) {
        int res = 0, num = 1;
        while (len > 0) {
            res = res * 10 + num;
            len--;
            num++;
        }
        return res;
    }

    // 当前长度的递增值
    private int gain(int len) {
        int res = 0;
        while (len > 0) {
            res = res * 10 + 1;
            len--;
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：36 MB, 在所有 Java 提交中击败了56.79%的用户。

## 官方解题

&emsp;官方思路暴力枚举每个顺次数并判断，由于顺次数只有少两个，故可行。
