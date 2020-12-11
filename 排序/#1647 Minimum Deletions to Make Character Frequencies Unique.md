[toc]

A string `s` is called **good** if there are no two different characters in `s` that have the same **frequency**.

Given a string `s`, return the **minimum** number of characters you need to delete to make `s` **good**.

The **frequency** of a character in a string is the number of times it appears in the string. For example, in the string `"aab"`, the **frequency** of `'a'` is $2$, while the **frequency** of `'b'` is $1$.

 

**Example 1**:

```
Input: s = "aab"
Output: 0
Explanation: s is already good.
```

**Example 2**:

```
Input: s = "aaabbbcc"
Output: 2
Explanation: You can delete two 'b's resulting in the good string "aaabcc".
Another way it to delete one 'b' and one 'c' resulting in the good string "aaabbc".
```

**Example 3**:

```
Input: s = "ceabaacb"
Output: 2
Explanation: You can delete both 'c's resulting in the good string "eabaab".
Note that we only care about characters that are still in the string at the end (i.e. frequency of 0 is ignored).
```



**Constraints**:

* $1 \le \text{s.length} \le 10^5$
* `s` contains only lowercase English letters.



## 题目解读

&emsp;规定字符串中不能出现两个字符频率一样，求得到合法字符的最小删除数目。

```java
class Solution {
    public int minDeletions(String s) {

    }
}
```

## 程序设计

* 统计字符数目，排序判断。

```java
class Solution {
    public int minDeletions(String s) {
        if (s == null || s.isEmpty()) return 0;

        int[] counter = new int[26];
        for (char c : s.toCharArray()) counter[c - 'a']++;

        Arrays.sort(counter);
        // 当前的上限
        int limit = counter[25] - 1, res = 0;
        for (int i = 24; i >= 0 && counter[i] > 0; i--) {
            if (counter[i] > limit) res += counter[i] - limit;
            // 更新下一个的上限
            limit = Math.max(Math.min(counter[i], limit) - 1, 0);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：9 ms, 在所有 Java 提交中击败了97.54%的用户

内存消耗：39.3 MB, 在所有 Java 提交中击败了60.80%的用户。

## 官方解题

&emsp;暂无，密切关注。
