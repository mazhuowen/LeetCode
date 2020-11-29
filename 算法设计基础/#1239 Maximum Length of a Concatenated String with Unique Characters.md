[toc]

Given an array of strings `arr`. String `s` is a concatenation of a sub-sequence of `arr` which have **unique characters**.

Return the maximum possible length of `s`.



**Constraints**:

* $1 \le \text{arr.length} \le 16$
* $1 \le \text{arr[i].length} \le 26$
* `arr[i]` contains only lower case English letters.



## 题目解读

&emsp;给定一个字符串数组`arr`，字符串`s`是将`arr`某一子序列字符串连接所得的字符串，如果`s`中的每一个字符都只出现过一次，就是一个可行解。返回所有可行解`s`中最长的长度。

```java
class Solution {
    public int maxLength(List<String> arr) {

    }
}
```

## 程序设计

* 使用数组记录当前字符串字符占用情况，回溯超时，时间复杂度为$O(N!)$。

```java
class Solution {
    int count;

    public int maxLength(List<String> arr) {
        // 标记单词组合所占字符
        boolean[] visited = new boolean[26];
        Set<String> set = new HashSet<>(arr);

        maxLength(0, set, visited);
        return count;
    }

    private void maxLength(int len, Set<String> set, boolean[] visited) {
        count = Math.max(count, len);

        // 尝试
        Set<String> temp = new HashSet<>(set);

        for (String t : temp) {
            // 字符有冲突
            if (!isValid(t, visited)) continue;

            set.remove(t);
            maxLength(len + t.length(), set, visited);

            // 回溯
            set.add(t);
            for (char c : t.toCharArray()) {
                visited[c - 'a'] = false;
            }
        }
    }

    private boolean isValid(String t, boolean[] visited) {
        int idx = 0;
        for (; idx < t.length(); idx++) {
            if (visited[t.charAt(idx) - 'a']) break;
            visited[t.charAt(idx) - 'a'] = true;
        }
        if (idx == t.length()) return true;
        // 恢复标识
        for (int i = 0; i < idx; i++) {
            visited[t.charAt(i) - 'a'] = false;
        }
        return false;
    }
}
```

* 仔细分析，题目求解最长长度，对字符串内部字符拼接顺序不作要求，前面从集合一个个遍历尝试所有组合显然过于冗余；如果按照数组遍历尝试，则大大降低时间复杂度。

```java
class Solution {
    int count;

    public int maxLength(List<String> arr) {
        // 标记单词组合所占字符
        boolean[] visited = new boolean[26];

        maxLength(0, arr, -1,  visited);
        return count;
    }

    // len为当前句子长度，start为起始尝试单词索引
    private void maxLength(int len, List<String> arr, int start, boolean[] visited) {
        count = Math.max(count, len);

        for (int i = start + 1; i < arr.size(); i++) {
            String t = arr.get(i);
            // 字符有冲突
            if (!isValid(t.toCharArray(), visited)) continue;
			// 尝试
            maxLength(len + t.length(), arr, i, visited);
            // 回溯
            for (char c : t.toCharArray()) {
                visited[c - 'a'] = false;
            }
        }
    }

    private boolean isValid(char[] str, boolean[] visited) {
        int idx = 0;
        // 判断是否存在重复（包括字符串间和字符串内部）
        for (; idx < str.length; idx++) {
            if (visited[str[idx] - 'a']) break;
            visited[str[idx] - 'a'] = true;
        }
        if (idx == str.length) return true;
        // 存在重复，恢复标识
        for (int i = 0; i < idx; i++) {
            visited[str[i] - 'a'] = false;
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(2^N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了84.46%的用户。

内存消耗：39MB，在所有java提交中击败了16.67%的用户。

## 官方解题

&emsp;暂无，密切关注。社区不采用数组记录，而是采用整数的比特位来记录26个字母。