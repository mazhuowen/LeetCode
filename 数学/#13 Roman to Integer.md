[toc]

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

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

* `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
* `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
* `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from `1` to `3999`.



## 题目解读

&emsp;将罗马数字转化为十进制数字。

```java
class Solution {
    public int romanToInt(String s) {

    }
}
```

## 程序设计

* 最简单的思路是维护映射对，遍历转换。

```java
class Solution {
    private static Map<Character, Integer> roman = new HashMap<>();
    static {
        roman.put('I', 1);
        roman.put('V', 5);
        roman.put('X', 10);
        roman.put('L', 50);
        roman.put('C', 100);
        roman.put('D', 500);
        roman.put('M', 1000);
    }

    public int romanToInt(String s) {
        int num = 0;
        char pre = '#';
        for(char c : s.toCharArray()) {
            // 遇到后面的值比前面的值大的情况，对应I在V或X前面的情况，减去
            if(pre != '#' && roman.get(pre) < roman.get(c)) {
                num += roman.get(c) - 2 * roman.get(pre);
            } else {
                num += roman.get(c);
            }
            pre = c;
        }
        return num;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了70.77%的用户。

内存消耗：41.1MB，在所有java提交中击败了5.01%的用户。

## 官方解题

&emsp;暂无，社区思路相近。