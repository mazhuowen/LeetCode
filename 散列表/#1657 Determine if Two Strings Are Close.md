[toc]

Two strings are considered **close** if you can attain one from the other using the following operations:

* Operation 1: Swap any two **existing** characters.
  * For example, `abcde -> aecdb`
* Operation 2: Transform **every** occurrence of one **existing** character into another **existing** character, and do the same with the other character.
  * For example, `aacabb -> bbcbaa` (all `a`'s turn into `b`'s, and all `b`'s turn into `a`'s)

You can use the operations on either string as many times as necessary.

Given two strings, `word1` and `word2`, return `true` if `word1` and `word2` are **close**, and `false` otherwise.

 

**Example 1**:

```
Input: word1 = "abc", word2 = "bca"
Output: true
Explanation: You can attain word2 from word1 in 2 operations.
Apply Operation 1: "abc" -> "acb"
Apply Operation 1: "acb" -> "bca"
```

**Example 2**:

```
Input: word1 = "a", word2 = "aa"
Output: false
Explanation: It is impossible to attain word2 from word1, or vice versa, in any number of operations.
```

**Example 3**:

```
Input: word1 = "cabbba", word2 = "abbccc"
Output: true
Explanation: You can attain word2 from word1 in 3 operations.
Apply Operation 1: "cabbba" -> "caabbb"
Apply Operation 2: "caabbb" -> "baaccc"
Apply Operation 2: "baaccc" -> "abbccc"
```

**Example 4**:

```
Input: word1 = "cabbba", word2 = "aabbss"
Output: false
Explanation: It is impossible to attain word2 from word1, or vice versa, in any amount of operations.
```



**Constraints**:

* $1 \le \text{word1.length, word2.length} \le 10^5$
* `word1` and `word2` contain only lowercase English letters.



## 题目解读

&emsp;定义相近的两个字符串为字符串可以相互转换，给定两个字符串，判断是否相近。

```java
class Solution {
    public boolean closeStrings(String word1, String word2) {

    }
}
```

## 程序设计

* 第一个操作任意交换两个字符的位置，第二个操作是将字符串中两类字符互换；第一个操作只改变字符的位置，不改变字符的种类和数量；第二个操作改变字符的位置和数量，但不改变字符的种类；
* 由上述分析可知两个操作均不会改变字符种类，其次两个操作也不改变字符的计数分布；这样可分别统计字符串，然后根据种类和计数分布判断。

```java
class Solution {
    public boolean closeStrings(String word1, String word2) {
        if (word1 == null || word2 == null || word1.length() != word2.length()) return false;

        int[] counter1 = new int[26], counter2 = new int[26];
        for (char c : word1.toCharArray()) {
            counter1[c - 'a']++;
        }
        for (char c : word2.toCharArray()) {
            counter2[c - 'a']++;
        }
        
        // 判断种类是否一致
        for (int i = 0; i < 26; i++) {
            if (counter1[i] == 0 && counter2[i] != 0 || counter1[i] != 0 && counter2[i] == 0) return false;
        }

        // 判断计数分布是否一致（字符数目可能改变，故需要排序，分布不变）
        Arrays.sort(counter1);
        Arrays.sort(counter2);
        for (int i = 0; i < 26; i++) {
            if (counter1[i] != counter2[i]) return false;
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：11 ms, 在所有 Java 提交中击败了98.44%的用户

内存消耗：39.3 MB, 在所有 Java 提交中击败了57.65%的用户。

## 官方解题

&emsp;暂无，密切关注。