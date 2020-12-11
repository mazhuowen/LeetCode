[toc]

You are given a string containing only 4 kinds of characters `'Q'`, `'W'`, `'E'` and `'R'`.

A string is said to be **balanced** if each of its characters appears $n/4$ times where $n$ is the length of the string.

Return the minimum length of the substring that can be replaced with **any** other string of the same length to make the original string `s` **balanced**.

Return $0$ if the string is already **balanced**.

 

**Example 1**:

```
Input: s = "QWER"
Output: 0
Explanation: s is already balanced.
```

**Example 2**:

```
Input: s = "QQWE"
Output: 1
Explanation: We need to replace a 'Q' to 'R', so that "RQWE" (or "QRWE") is balanced.
```

**Example 3**:

```
Input: s = "QQQW"
Output: 2
Explanation: We can replace the first "QQ" to "ER". 
```

**Example 4**:

```
Input: s = "QQQQ"
Output: 3
Explanation: We can replace the last 3 'Q' to make s = "QWER".
```



**Constraints**:

* $1 \le \text{s.length} \le 10^5$
* `s.length` is a multiple of $4$
* `s` contains only `'Q'`, `'W'`, `'E'` and `'R'`.



## 题目解读

&emsp;字符串只能有四个字符，求替换使得字符串四个字符计数相等的最小子字符串长度。

```java
class Solution {
    public int balancedString(String s) {
        
    }
}
```

## 程序设计

* 由于最后各个字符频数相等，故贪婪的不考虑连续性，只需将频数高于平均值的部分替换即可；
* 考虑连续性，则问题转换为包含需要替换的频数的最短队列。

```java
class Solution {
    public int balancedString(String s) {
        if (s == null || s.length() % 4 != 0) throw new IllegalArgumentException("invalid param");
        char[] seq = s.toCharArray();
        // 计数
        int[] counter = new int[4];
        for (char c : seq) count(c, 1, counter);
        // 已经是平衡的
        if (counter[0] == counter[1] && counter[1] == counter[2] && counter[2] == counter[3]) return 0;
        
        // 基准
        int base = seq.length / 4;
        int res = seq.length, left = 0, right = 0;
        while (right < seq.length) {
            // 计数减少
            count(seq[right++], -1, counter);
            // 当前队列符合要求，继续收缩判断
            while (left < right && hasEnoughChar(base, counter)) {
                // 更新长度
                res = Math.min(res, right - left);
                count(seq[left++], 1, counter);
            }
        }

        return res;
    }

    private void count(char c, int inc, int[] counter) {
        if (c == 'Q') counter[0] += inc;
        else if (c == 'W') counter[1] += inc;
        else if (c == 'E') counter[2] += inc;
        else counter[3] += inc;
    }

    // 判断剩余的字符数是否小于等于基数
    private boolean hasEnoughChar(int base, int[] counter) {
        for (int num : counter) if (num > base) return false;
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：11 ms, 在所有 Java 提交中击败了33.14%的用户。

内存消耗：38.6 MB, 在所有 Java 提交中击败了66.86%的用户。

## 官方解题

&emsp;暂无，密切关注。
