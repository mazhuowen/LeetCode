[toc]

From any string, we can form a subsequence of that string by deleting some number of characters (possibly no deletions).

Given two strings `source` and `target`, return the minimum number of subsequences of `source` such that their concatenation equals `target`. If the task is impossible, return $-1$.

 

**Example 1**:

```
Input: source = "abc", target = "abcbc"
Output: 2
Explanation: The target "abcbc" can be formed by "abc" and "bc", which are subsequences of source "abc".
```

**Example 2**:

```
Input: source = "abc", target = "acdbc"
Output: -1
Explanation: The target string cannot be constructed from the subsequences of source string due to the character "d" in target string.
```

**Example 3**:

```
Input: source = "xyz", target = "xzyxz"
Output: 3
Explanation: The target string can be constructed as follows "xz" + "y" + "xz".
```



**Constraints**:

* Both the `source` and `target` strings consist of only lowercase English letters from "a"-"z".
* The lengths of `source` and `target` string are between $1$ and $1000$.



## 题目解读

&emsp;给定目标字符串，求可由源字符串子序列组成的数目。

```java
class Solution {
    public int shortestWay(String source, String target) {

    }
}
```

## 程序设计

* 最基本的思路是根据每个字符统计在匹配字符串中出现的索引，记录之前匹配到的索引，这样遍历目标字符串，对于当前字符查找比之前匹配索引大的最小索引，如果存在则匹配；如果不存在，表示当前子序列无法满足，必须折返到匹配字符串起始位置，此时数目加一；
* 最后还需加上未匹配完的数目。

```java
class Solution {
    public int shortestWay(String source, String target) {
        List<Integer>[] index = new List[26];
        // 统计每个字符的索引
        for (int i = 0; i < 26; i++) index[i] = new ArrayList<>();
        for (int i = 0; i < source.length(); i++) index[source.charAt(i) - 'a'].add(i);

        int res = 0, preIdx = -1;
        for (char c : target.toCharArray()) {
            // 不存在字符，返回-1
            if (index[c - 'a'].isEmpty()) return -1;
            // 查找在preIdx之后的第一个索引
            int idx = binarySearch(preIdx, index[c - 'a'], 0, index[c - 'a'].size());
            // 不存在比preIdx大的索引，即一个子序列匹配完成，定位到起始位置
            if (idx == index[c - 'a'].size()) {
                res++;
                preIdx = index[c - 'a'].get(0);
            } 
            // 更新preIdx
            else {
                preIdx = index[c - 'a'].get(idx);
            }
        }
        // 最后未结束的序列加一
        return ++res;
    }

    private int binarySearch(int target, List<Integer> arr, int left, int right) {
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (arr.get(mid) > target) right = mid;
            else left = mid + 1;
        }
        return left;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2M)$，空间复杂度为$O(M)$，$M$为匹配串长度，$N$为目标串长度。

执行用时：5 ms, 在所有 Java 提交中击败了56.46%的用户

内存消耗：36.8 MB, 在所有 Java 提交中击败了35.62%的用户。

## 官方解题

&emsp;暂无，密切关注。