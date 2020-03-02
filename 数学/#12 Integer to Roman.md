[toc]

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

| Symbol | Value |
| ------ | ----- |
| I      | 1     |
| V      | 5     |
| X      | 10    |
| L      | 50    |
| C      | 100   |
| D      | 500   |
| M      | 1000  |

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X + II`. The number twenty seven is written as `XXVII`, which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

* I can be placed before V (5) and X (10) to make 4 and 9. 
* X can be placed before L (50) and C (100) to make 40 and 90. 
* C can be placed before D (500) and M (1000) to make 400 and 900.

Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.



## 题目解读

&emsp;给定整数，转化为罗马数字表示。

```java
class Solution {
    public String intToRoman(int num) {

    }
}
```

## 程序设计

* 对4和9的特殊情况进行判断处理，对大于5、小于5的常规情况进行拼装。

```java
class Solution {
    public String intToRoman(int num) {
        String res = "";
        // 记录位数
        int count = 0;
        // 记录个位数
        int val;
        while(num > 0) {
            val = num % 10;
            count++;
            num /= 10;
            // 4和9特殊值处理
            if(val == 4) {
                if(count == 1) res = "IV" + res;
                else if(count == 2) res = "XL" + res;
                else if(count == 3) res = "CD" + res;
            } else if(val == 9) {
                if(count == 1) res = "IX" + res;
                else if(count == 2) res = "XC" + res;
                else if(count == 3) res = "CM" + res;
            } 
            // 大于5的值处理
            else if(val >= 5) {
                for(int i = val - 5; i > 0; i--) {
                    if(count == 1) res = "I" + res;
                    else if(count == 2) res = "X" + res;
                    else if(count == 3) res = "C" + res;
                    else if(count == 4) res = "M" + res;
                }
                if(count == 1) res = "V" + res;
                else if(count == 2) res = "L" + res;
                else if(count == 3) res = "D" + res;
            } 
            // 小于5的值处理
            else {
                for(int i = val; i > 0; i--) {
                    if(count == 1) res = "I" + res;
                    else if(count == 2) res = "X" + res;
                    else if(count == 3) res = "C" + res;
                    else if(count == 4) res = "M" + res;
                }
            }
        } 
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：8ms，在所有java提交中击败了23.74%的用户。

内存消耗：41.6MB，在所有java提交中击败了5.01%的用户。

## 官方解题

&emsp;暂无，密切关注。社区较好的方法采用贪心算法：

```java
public class Solution {

    public String intToRoman(int num) {
        // 把阿拉伯数字与罗马数字可能出现的所有情况和对应关系，放在两个数组中
        // 并且按照阿拉伯数字的大小降序排列，这是贪心选择思想
        int[] nums = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        String[] romans = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

        StringBuilder stringBuilder = new StringBuilder();
        int index = 0;
        while (index < 13) {
            // 特别注意：这里是等号
            while (num >= nums[index]) {
                // 注意：这里是等于号，表示尽量使用大的"面值"
                stringBuilder.append(romans[index]);
                num -= nums[index];
            }
            index++;
        }
        return stringBuilder.toString();
    }
}
```

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：5ms，在所有java提交中击败了90.91%的用户。

内存消耗：40.9MB，在所有java提交中击败了5.01%的用户。