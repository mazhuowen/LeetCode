[toc]

Return the lexicographically smallest subsequence of `text` that contains all the distinct characters of `text` exactly once.



## 题目解读

&emsp;给定字符串，求不重复且字典序最小的子序列。

```java
class Solution {
    public String smallestSubsequence(String text) {

    }
}
```

## 程序设计

* 同[#316 Remove Duplicate Letters](./#316 Remove Duplicate Letters.md)。

```java
class Solution {
    public String smallestSubsequence(String text) {
        if (text == null || text.isEmpty()) return text;

        char[] s = text.toCharArray();
        // 记录字符最后位置
        int[] lastIdx = new int[26];
        Arrays.fill(lastIdx, -1);
        for (int i = 0; i < s.length; i++) {
            lastIdx[s[i] - 'a'] = i;
        }
		// 记录是否包含在栈中
        boolean[] record = new boolean[26];
        
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length; i++) {
            int idx = s[i] - 'a';
            // 栈中已存在，此时在正确位置，丢弃本次字符
            if (record[idx]) continue;
            //　字符不是升序，则遍历出栈后续还会出现的字符
            while (!stack.isEmpty() && stack.peek() > s[i]
                    && lastIdx[stack.peek() - 'a'] > i) {
                record[stack.pop() - 'a'] = false;
            }
            // 入栈当前字符
            stack.push(s[i]);
            record[idx] = true;
        }

        StringBuffer sb = new StringBuffer();
        while (!stack.isEmpty()) sb.insert(0, stack.pop());
        return sb.toString();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：3ms，在所有java提交中击败了95.64%的用户。

内存消耗：37.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同[#316 Remove Duplicate Letters](./#316 Remove Duplicate Letters.md)。