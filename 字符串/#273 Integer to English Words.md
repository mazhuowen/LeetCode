[toc]

Convert a non-negative integer to its english words representation. Given input is guaranteed to be less than $2^{31} - 1$.



## 题目解读

&emsp;将数字转化为英文描述。

```java
class Solution {
    public String numberToWords(int num) {
        
    }
}
```

## 程序设计

* 注意到`1234567891`描述为`One Billion Two Hundred Thirty Four Million Five Hundred Sixty Seven Thousand Eight Hundred Ninety One`，可以拆分为`1 Billion 234 Million 567 Thousand 891`，可见是每三位为一个循环，只需加上不同的单位。
* 对于三位数字，最高位需要加上`Hundred`单位；剩下的两位需要判断是否是十到二十之间的数，是的话为一个单词，否则为两个单词的组合。

```java
class Solution {
    // 个位数（除了0都有空格）
    private final static String[] singleDigit = new String[]{"Zero", "One ", "Two ", "Three ", "Four ", "Five ", "Six ", "Seven ", "Eight ", "Nine "}; 
    // 十位数（有空格）
    private final static String[] tenDigit1 = new String[]{"Ten ", "Eleven ", "Twelve ", "Thirteen ", "Fourteen ", "Fifteen ", "Sixteen ", "Seventeen ", "Eighteen ", "Nineteen "}; 
    // 十位数（有空格）
    private final static String[] tenDigit2 = new String[]{"Twenty ", "Thirty ", "Forty ", "Fifty ", "Sixty ", "Seventy ", "Eighty ", "Ninety "}; 
    // 单位（除了个位，有空格）
    private final static String[] unit = new String[]{"", "Thousand ", "Million ", "Billion ", "Hundred "};

    public String numberToWords(int num) {
        if (num < 0) throw new IllegalArgumentException("invalid param");
        // 0为特殊情况
        if (num == 0) return singleDigit[0];
        
        StringBuffer sb = new StringBuffer();
        // 每三位一组
        for (int i = 0; i < 4 && num > 0; i++) {
            sb.insert(0, numberToWords(num % 1000, i));
            num /= 1000;
        }
        // 去除最后的空格
        return sb.toString().substring(0, sb.length() - 1);
    }

    private String numberToWords(int num, int idx) {
        // 为0则返回空
        if (num == 0) return "";
        // 拼接单位
        StringBuffer sb = new StringBuffer(unit[idx]);
        int cur = num % 100;
        // 十位和个位组合以1开头
        if (cur > 9 && cur < 20) {
            sb.insert(0, tenDigit1[cur - 10]);
        } 
        // 十位大于2或个位大于0
        else {
            if (cur % 10 > 0) sb.insert(0, singleDigit[cur % 10]);
            if (cur / 10 > 0) sb.insert(0, tenDigit2[cur / 10 - 2]);
        }
        // 百位拼接
        num /= 100;
        if (num > 0) {
            // 拼接单位
            sb.insert(0, unit[4]);
            sb.insert(0, singleDigit[num]);
        }
        return sb.toString();
    }

}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$，$N$为数字位数。

执行用时：3ms，在所有java提交中击败了83.58%的用户。

内存消耗：37.7MB, 在所有java提交中击败了20.00%的用户。

## 官方解题

&emsp;思路同上，实现略有差异。