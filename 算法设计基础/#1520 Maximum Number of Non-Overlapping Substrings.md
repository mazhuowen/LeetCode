[toc]

Given a string `s` of lowercase letters, you need to find the maximum number of **non-empty** substrings of `s` that meet the following conditions:

* The substrings do not overlap, that is for any two substrings `s[i..j]` and `s[k..l]`, either $j < k$ or $i > l$ is true.
* A substring that contains a certain character `c` must also contain all occurrences of `c`.

Find the maximum number of substrings that meet the above conditions. If there are multiple solutions with the same number of substrings, return the one with minimum total length. It can be shown that there exists a unique solution of minimum total length.

Notice that you can return the substrings in **any** order.



**Constraints**:

* $1 \le \text{s.length} \le 10^5$
* `s` contains only lowercase English letters.



## 题目解读

&emsp;给定字符串，找到最多的非空子字符串，子字符串之间互不重叠，且一个子字符中有字符`c`，则`s`中所有的`c`都应该在该子字符串中。如果有多个子字符串组合数目相等，则返回总长度最小的解。

```java
class Solution {
    public List<String> maxNumOfSubstrings(String s) {
    
    }
}
```

## 程序设计

* 第一影响是使用回溯暴力遍历所有组合，注意题目中子字符串必须包含所有字符，可统计每个字符在`s`中出现的首、未位置，通过扩展的方式来切分，最终返回最短的组合。当字符串较长时，该方法会超时。

```java
class Solution {
    int minLen = Integer.MAX_VALUE;
    List<String> res;
    
    public List<String> maxNumOfSubstrings(String s) {
        res = new ArrayList<>();
        if (s == null || s.isEmpty()) return res;
        char[] strs = s.toCharArray();
        // 统计首末位置
        int[] last = new int[26], first = new int[26];
        for (int i = 0; i < strs.length; i++) {
            last[strs[i] - 'a'] = i;
        }
        for (int i = strs.length - 1; i >= 0; i--) {
            first[strs[i] - 'a'] = i;
        }
        
        // 回溯
        backTracing(strs, 0, last, first, new ArrayList<>(), 0);
        return res;
    }
    
    private void backTracing(char[] strs, int start, int[] last, int[] first, List<String> path, int len) {
        if (res.size() < path.size() || res.size() == path.size() && len < minLen) {
            res = new ArrayList<>(path);
            minLen = len;
        }
        
        // 开始字符
        breakpoint: for (int i = start; i < strs.length; i++) {
            if (first[strs[i] - 'a'] < i) continue;
            // 结束字符
            int end = i;
            for (int j = i; j <= end; j++) {
                if (first[strs[j] - 'a'] < i) continue breakpoint;
                end = Math.max(end, last[strs[j] - 'a']);
            }
            // 试探
            path.add(new String(strs, i, end + 1 - i));
            backTracing(strs, end + 1, last, first, path, len + end + 1 - i);
            // 回溯
            path.remove(path.size() - 1);
        }
    }
}
```

* 换个思路，由于字符串都是由$26$个小写字母组成的，即在该字符串中所有满足条件的子字符串不超过$26$个，可以不考虑覆盖，先将所有子字符串罗列出来，然后再判断是否覆盖并拼接。

```java
class Solution {
    public List<String> maxNumOfSubstrings(String s) {
        List<String> res = new LinkedList<>();
        if (s == null || s.isEmpty()) return res;

        char[] strs = s.toCharArray();
        // 统计首、末位置
        int[] last = new int[26], first = new int[26];
        Arrays.fill(last, -1);
        Arrays.fill(first, -1);
        for (int i = 0; i < strs.length; i++) {
            int idx = strs[i] - 'a';
            if (first[idx] == -1) first[idx] = i;
            last[idx] = i;
        }

        List<Interval> candidate = new ArrayList<>();
        // 对26个字符开头的子串遍历扩展
        point: for (int c = 0; c < 26; c++) {
            int left = first[c], right = last[c];
            if (left == -1) continue;
            // 扩展
            for (int i = left + 1; i <= right; i++) {
                // left开头的子字符串包含在前面出现过的字符，无法构成符合要求的子字符串，重新遍历下个字符开头的子字符串
                if (first[strs[i] - 'a'] < left) continue point;
                // 扩展延申
                right = Math.max(right, last[strs[i] - 'a']);
            }
            // 加入区间候选
            candidate.add(new Interval(left, right, new String(strs, left, right - left + 1)));
        }

        // 排序候选区间，贪心选择，根据结束区间排序，当结束区间一致时根据起始区间逆排序
        Collections.sort(candidate, (a, b) -> a.right == b.right ? b.left - a.left : a.right - b.right);
        // 当前右区间
        int right = -1;
        for (Interval i : candidate) {
            // 不覆盖，加入
            if (i.left > right) {
                res.add(i.s);
                right = i.right;
            }
        }
        return res;
    }
}

class Interval {
    int left;
    int right;
    String s;

    Interval(int left, int right, String s) {
        this.left = left;
        this.right = right;
        this.s = s;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：12ms，在所有java提交中击败了96.80%的用户。

内存消耗：40.7MB，在所有java提交中击败了84.62%的用户。

## 官方解题

&emsp;思路同上。