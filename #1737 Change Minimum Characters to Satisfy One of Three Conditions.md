[toc]

You are given two strings a and b that consist of lowercase letters. In one operation, you can change any character in a or b to any lowercase letter.

Your goal is to satisfy one of the following three conditions:

Every letter in a is strictly less than every letter in b in the alphabet.
Every letter in b is strictly less than every letter in a in the alphabet.
Both a and b consist of only one distinct letter.
Return the minimum number of operations needed to achieve your goal.

 

Example 1:

Input: a = "aba", b = "caa"
Output: 2
Explanation: Consider the best way to make each condition true:
1) Change b to "ccc" in 2 operations, then every letter in a is less than every letter in b.
2) Change a to "bbb" and b to "aaa" in 3 operations, then every letter in b is less than every letter in a.
3) Change a to "aaa" and b to "aaa" in 2 operations, then a and b consist of one distinct letter.
The best way was done in 2 operations (either condition 1 or condition 3).
Example 2:

Input: a = "dabadd", b = "cda"
Output: 3
Explanation: The best way is to make condition 1 true by changing b to "eee".


Constraints:

1 <= a.length, b.length <= 105
a and b consist only of lowercase letters.



## 题目解读

&emsp;

```java
class Solution {
    public int minCharacters(String a, String b) {

    }
}
```

## 程序设计

* 

```java
class Solution {
    public int minCharacters(String a, String b) {
        // 统计
        int[] ca = count(a), cb = count(b);
        int res = Integer.MAX_VALUE;
        // 尝试a-y作为切分点
        for (int i = 0; i < 25; i++) {
            res = Math.min(res, Math.min(equal(ca, cb, i), Math.min(less(ca, cb, i), less(cb, ca, i))));
        }
        // 等于还需要尝试z
        res = Math.min(res, equal(ca, cb, 25));
        return res;
    }
    
    private int[] count(String str) {
        int[] counter = new int[26];
        for (char c : str.toCharArray()) {
            counter[c - 'a']++;
        }
        return counter;
    }
    
    // a中小于等于target，b中大于target的花销
    private int less(int[] a, int[] b, int target) {
        int res = 0;
        for (int i = 0; i < 26; i++) {
            if (i <= target) res += b[i];
            else res += a[i];
        }
        return res;
    }
    
    // a、b由相等的字符构成的花销
    private int equal(int[] a, int[] b, int target) {
        int res = 0;
        for (int i = 0; i < 26; i++) {
            if (i != target) res += a[i] + b[i];
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(M + N)$，空间复杂度为$O(1)$。



## 官方解题

&emsp;

```java

```

&emsp;时间复杂度为$O()$，空间复杂度为$O()$。
