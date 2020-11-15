[toc]

Given two integers representing the numerator and denominator of a fraction, return the fraction in string format.

If the fractional part is repeating, enclose the repeating part in parentheses.



## 题目解读

&emsp;给定分子、分母，输出分数的字符串形式，对于小数部分的重复值使用括号表示。

```java
class Solution {
    public String fractionToDecimal(int numerator, int denominator) {

    }
}
```

## 程序设计

* 整数部分可以通过整形除法直接得到，此处需要注意负号的判断，因为如果分子小于分母，得到的结果是$0$，会丢失负号，故需要特别处理；其次需要注意溢出的情况，如果分母是最大值或最小值，由于在小数除法部分需要乘十操作，存在溢出的问题，故需要将整形转为长整形。
* 最后需要考虑的问题就是对循环除数的判断。可以记录之前的除数，如果重复，说明发生循环，以`4/333`为例，前三个除数分别为$4$、$40$、$67$，第四个除数是$4$，发生重复。

```java
class Solution {
    public String fractionToDecimal(int numerator, int denominator) {
        if(denominator == 0) return "";
        if(numerator == 0) return "0";
        
        StringBuffer sb = new StringBuffer();
        // 判断负数
        if(numerator < 0 ^ denominator < 0) sb.append("-"); 
        
        // 转化为long，避免溢出
        long remiander = Math.abs(Long.valueOf(numerator));
        long divisor  = Math.abs(Long.valueOf(denominator));
        // 整数部分
        sb.append(String.valueOf(remiander / divisor));
        remiander %= divisor;
        // 已被除尽
        if(remiander == 0) return sb.toString();
        
        // 插入小数号
        sb.append(".");
        // 记录除数，判断是否有循环产生
        Map<Long, Integer> record = new HashMap<>();
        // 没除尽则一直循环
        while (remiander != 0) {
            // 除数字典已包含，表示有循环结果
            if (record.containsKey(remiander)) {
                int idx = record.get(remiander);
                sb.insert(idx, "(");
                sb.append(")");
                return sb.toString();
            }
            // 拼接
            record.put(remiander, sb.length());
            sb.append(remiander  * 10 / divisor);
            remiander = remiander * 10 % divisor;
        }
        return sb.toString();
    }
}
```

## 性能分析

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.2MB，在所有java提交中击败了5.12%的用户。

## 官方解题

&emsp;同上。