[toc]

Given a string `S` and a string `T`, find the minimum window in `S` which will contain all the characters in `T` in complexity $O(n)$.



**Note**:

* If there is no such window in `S` that covers all characters in `T`, return the empty string `""`.
* If there is such window, you are guaranteed that there will always be only one unique minimum window in `S`.



## 题目解读

&emsp;给定一个字符串`S`、一个字符串`T`，请在字符串`S`里面找出：包含`T`所有字母的最小子串。

```java
class Solution {
    public String minWindow(String s, String t) {

    }
}
```

## 程序设计

* 由于检测目标是字符，可以一次遍历。首先需要哈希表计数目标字符串中的字符及其数目，则当前遍历的窗口必须包含这些字符且计数大于等于目标字符中的计数。
* 由于字符可以重复，入队策略为入队，再判断出队。

```java
class Solution {
    public String minWindow(String s, String t) {
        if(s == null || s.isEmpty() || t == null || t.isEmpty()) {
            return "";
        }
        // 对目标序列字符统计
        Map<Character, Integer> counter = new HashMap<>();
        for(char c : t.toCharArray()) {
            counter.put(c, counter.getOrDefault(c, 0) + 1);
        }
        String res = "";
        // 记录最小长度
        int minLen = Integer.MAX_VALUE;
        // 记录最小序列起始结束位置
        int start = 0, end = 0;
        while(end < s.length()) {
            // 入队当前元素（如果是T中的字符还需要计数减一）
            if(counter.get(s.charAt(end)) != null) {
                counter.put(s.charAt(end), counter.get(s.charAt(end)) - 1);
            }
            end++;
            // 删除队首T中不包含的字符及T中包含的字符但是超过T中的计数
            while(start < s.length() && (!counter.keySet().contains(s.charAt(start)) || counter.get(s.charAt(start)) < 0)) {
                // 如果是T中的字符，还需要计数加一
                if(counter.get(s.charAt(start)) != null) {
                    counter.put(s.charAt(start), counter.get(s.charAt(start)) + 1);
                }
                start++;
            }
            // 判断当前序列是否满足条件，遍历哈希表里的计数，检查是否有大于0的（即当前序列有字符不满足T中要求的数目）
            // 序列中字符数目可以超过T中的字符数目，但不能小于
            boolean flag = true;
            for(int val : counter.values()) {
                if(val > 0) {
                    flag = false;
                    break;
                }
            }
            // 符合要求且长度更小，更新字符串
            if(flag && minLen > end - start) {
                minLen = end - start;
                res = s.substring(start, end);
            }
        }
        return res;
    }
}
```

* 实际上还可以优化。上述代码循环体中还有一个对字典的循环判断，实际上可以在遍历的时候统计字符种数，如果当前字符达到目标字符串`T`中的数目，则种数加一。这样每次入队后只需要判断种数是否和`T`中的一致，一致则更新。

```java
class Solution {
    public String minWindow(String s, String t) {
        if(s == null || s.isEmpty() || t == null || t.isEmpty()) {
            return "";
        }
        // 对目标序列字符统计
        Map<Character, Integer> counter = new HashMap<>();
        for(char c : t.toCharArray()) {
            counter.put(c, counter.getOrDefault(c, 0) + 1);
        }
        String res = "";
        // 记录最小长度
        int minLen = Integer.MAX_VALUE;
        // 记录最小序列起始结束位置
        int start = 0, end = 0;
        // 记录T中的字符种数（AABC种数为3）
        int nums = 0;
        while(end < s.length()) {
            // 入队当前元素（如果是T中的字符还需要计数减一）
            if(counter.get(s.charAt(end)) != null) {
                int count = counter.get(s.charAt(end)) - 1;
                counter.put(s.charAt(end), count);
                // 计数为0表示序列中该字符已达到要求数目，种数加一（由于程序性质，后面该字符会一直维持在要求数目及以上）
                if(count == 0) {
                    nums++;
                }
            }
            end++;
            // 删除队首T中不包含的字符及T中包含的字符但是超过T中的计数
            while(start < s.length() && (!counter.keySet().contains(s.charAt(start)) || counter.get(s.charAt(start)) < 0)) {
                // 如果是T中的字符，还需要计数加一
                if(counter.get(s.charAt(start)) != null) {
                    counter.put(s.charAt(start), counter.get(s.charAt(start)) + 1);
                }
                start++;
            }
            // 当前序列字符种数已达到要求且长度比之前的序列短，则更新
            if(nums == counter.size() && minLen > end - start) {
                minLen = end - start;
                res = s.substring(start, end);
            }
        }
        return res;
    }
}
```

测试用例：待遍历字符串`aaaaaaaaaaaabbbbbcdd`，目标字符串`abcdd`，结果为`abbbbbcdd`。

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(M)$，其中$M$为字符串`T`中字符的种数。

执行用时：58ms，在所有java提交中击败了13.83%的用户。

内存消耗：43MB，在所有java提交中击败了6.97%的用户。

&emsp;优化后时间复杂度为$O(N)$，空间复杂度为$O(M)$。

执行用时：22ms，在所有java提交中击败了59.26%的用户。

内存消耗：40.4MB，在所有java提交中击败了12.14%的用户。

## 官方解题

&emsp;思路类似。社区中时间性能较快的方法不使用集合类，而是采用数组。