[toc]

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Write a function to determine if a number is strobogrammatic. The number is represented as a string.



## 题目解读

&emsp;判断字符串是不是中心对称数。

```java
class Solution {
    public boolean isStrobogrammatic(String num) {

    }
}
```

## 程序设计

* 简单的思路，保存映射关系，遍历对比。

```java
class Solution {
    public boolean isStrobogrammatic(String num) {
        if(num == null || num.isEmpty()) {
            return true;
        }
        Map<Character, Character> record = new HashMap<>();
        record.put('0', '0');
        record.put('1', '1');
        record.put('6', '9');
        record.put('8', '8');
        record.put('9', '6');
        for(int i = 0; i <= num.length() / 2; i++) {
            // 当前字符是字典以外的字符或转换后不对应，则不合法
            if(record.get(num.charAt(i)) == null || record.get(num.charAt(i)) != num.charAt(num.length() - 1 - i)) {
                return false;
            }
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.5MB，在所有java提交中击败了5.66%的用户。

## 官方解题

&emsp;暂无，密切关注。