[toc]

Given a string `s`, a k duplicate removal consists of choosing $k$ adjacent and equal letters from `s` and removing them causing the left and the right side of the deleted substring to concatenate together.

We repeatedly make $k$ duplicate removals on `s` until we no longer can.

Return the final string after all such duplicate removals have been made.

It is guaranteed that the answer is unique.

 

**Example 1**:

```
Input: s = "abcd", k = 2
Output: "abcd"
Explanation: There's nothing to delete.
```

**Example 2**:

```
Input: s = "deeedbbcccbdaa", k = 3
Output: "aa"
Explanation: 
First delete "eee" and "ccc", get "ddbbbdaa"
Then delete "bbb", get "dddaa"
Finally delete "ddd", get "aa"
```

**Example 3**:

```
Input: s = "pbbcggttciiippooaais", k = 2
Output: "ps"
```



**Constraints**:

* $1 \le \text{s.length} \le 10^5$
* $2 \le k \le 10^4$
* `s` only contains lower case English letters.



## 题目解读

&emsp;删除字符串中相邻$k$个相等字符的子串，直到不存在连续相邻$k$个字符相等。

```java
class Solution {
    public String removeDuplicates(String s, int k) {

    }
}
```

## 程序设计

* 最基本的思路就是递归删除。

```java
class Solution {
    public String removeDuplicates(String s, int k) {
        int idx = 0;
        char[] seq = s.toCharArray();
        for (int i = 0; i < seq.length; i++) {
            // 找到相邻的三个中等字符，消去
            if (idx >= k - 1 && isSame(seq[i], seq, idx, k)) {
                // 回退
                idx -= k - 1;
            } else {
                seq[idx++] = seq[i];
            }
        }
        // 不存在三个相等的字符，返回
        if (idx == seq.length) return s;
        return removeDuplicates(new String(seq, 0, idx), k);
    }

    private boolean isSame(char c, char[] seq, int idx, int k) {
        for (int i = 1; i < k; i++) {
            if (seq[idx - i] != c) return false;
        }
        return true;
    }
}
```

* 可使用栈计数当前字符数目

```java
class Solution {
    public String removeDuplicates(String s, int k) {
        int idx = 0;
        Char[] stack = new Char[s.length()];
        for (char c : s.toCharArray()) {
            // 进栈
            if (idx == 0 || stack[idx - 1].c != c) stack[idx++] = new Char(c, 1);
            // 达到k个，弹出
            else if (idx > 0 && ++stack[idx - 1].count == k) idx--;
        }
        
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < idx; i++) {
            buildStr(sb, stack[i].c, stack[i].count);
        }
        return sb.toString();
    }

    private void buildStr(StringBuffer sb, char c, int count) {
        for (int i = 0; i < count; i++) sb.append(c);
    }
}

class Char {
    char c;
    int count;

    Char(char c, int count) {
        this.c = c;
        this.count = count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2/K)$，空间复杂度为$O(N/K)$。

执行用时：789 ms, 在所有 Java 提交中击败了5.18%的用户。

内存消耗：38.6 MB, 在所有 Java 提交中击败了90.81%的用户。

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：4 ms, 在所有 Java 提交中击败了96.07%的用户

内存消耗：38.5 MB, 在所有 Java 提交中击败了93.02%的用户。

## 官方解题

&emsp;同上。