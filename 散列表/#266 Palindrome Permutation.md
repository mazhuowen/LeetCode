[toc]

Given a string, determine if a permutation of the string could form a palindrome.



## 题目解读

&emsp;给定一个字符串，判断该字符串中是否可以通过重新排列组合，形成一个回文字符串。

```java
class Solution {
    public boolean canPermutePalindrome(String s) {

    }
}
```

## 程序设计

* 初步思路是字符串长度为偶数，则字符串所有字符的计数应该为偶数，如果字符串的长度是奇数，则只有一个字符计数是奇数，其它都是偶数。

```java
class Solution {
    public boolean canPermutePalindrome(String s) {
        if(s == null || s.isEmpty()) {
            return true;
        }
        // 奇数为1偶数为0
        int odd = s.length() & 1;
        // 保存计数
        Map<Character, Integer> counter = new HashMap<>();
        for(char c : s.toCharArray()) {
            counter.put(c, counter.getOrDefault(c, 0) + 1);
        }
        // 检查
        for(int val : counter.values()) {
            if((val & 1) == 1) {
                odd--;
            }
        }
        return odd == 0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了72.31%的用户。

内存消耗：37.6MB，在所有java提交中击败了5.40%的用户。

## 官方解题

&emsp;官方除了上述思路还提供了数组代替字典、集合代替字典的思路，整体思路相同。