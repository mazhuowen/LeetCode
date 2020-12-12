[toc]

Your friend is typing his `name` into a keyboard. Sometimes, when typing a character `c`, the key might get long pressed, and the character will be typed $1$ or more times.

You examine the `typed` characters of the keyboard. Return `True` if it is possible that it was your friends name, with some characters (possibly none) being long pressed.

 

**Example 1**:

```
Input: name = "alex", typed = "aaleex"
Output: true
Explanation: 'a' and 'e' in 'alex' were long pressed.
```

**Example 2**:

```
Input: name = "saeed", typed = "ssaaedd"
Output: false
Explanation: 'e' must have been pressed twice, but it wasn't in the typed output.
```

**Example 3**:

```
Input: name = "leelee", typed = "lleeelee"
Output: true
```

**Example 4**:

```
Input: name = "laiden", typed = "laiden"
Output: true
Explanation: It's not necessary to long press any character.
```



**Constraints**:

* $1 \le \text{name.length} \le 1000$
* $1 \le \text{typed.length} \le 1000$
* `name` and `typed` contain only lowercase English letters.



## 题目解读

&emsp;检测目标字符串是否是姓名的叠影输入。

```java
class Solution {
    public boolean isLongPressedName(String name, String typed) {

    }
}
```

## 程序设计

* 采用贪婪思路匹配，以`leelee`和`lleeelee`为例，若当前字符匹配，则索引进入下一个字符，若当前字符不匹配，则判断索引的前一个字符是否匹配。

```java
class Solution {
    public boolean isLongPressedName(String name, String typed) {
        int idx = 0;
        char[] seq = name.toCharArray();
        for (char c : typed.toCharArray()) {
            // 与当前或前一个字符是否匹配
            if (idx < seq.length && c == seq[idx] ||  idx > 0 && c == seq[--idx]) idx++;
            else return false;
        }
        return idx == seq.length;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N + M)$，空间复杂度为$O(N + M)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：36.8 MB, 在所有 Java 提交中击败了6.68%的用户。

## 官方解题

&emsp;官方思路类似，双指针遍历判断。
