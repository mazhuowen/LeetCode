[toc]

Given an array `A` of strings, find any smallest string that contains each string in `A` as a substring.

We may assume that no string in `A` is substring of another string in `A`.



**Note**:

* $1 \le \text{A.length} \le 12$
* $1 \le \text{A[i].length} \le 20$



## 题目解读

&emsp;给定字符串数组，得出最短超级字符串，超级字符串表示包含所有字符串的字符串。

```java
class Solution {
    public String shortestSuperstring(String[] A) {

    }
}
```

## 程序设计

* 最基本的思路是暴力回溯，遍历当前字符串，如果当前字符串与已拼接字符串结尾有重合，则截取拼接；最后选择最短的字符串。时间复杂度为$O(N!M)$，$M$为字符串平均长度，会超时。

```java
class Solution {
    String minStr;

    public String shortestSuperstring(String[] A) {
        if (A == null || A.length == 0) return "";

        boolean[] visited = new boolean[A.length];
        for (int i = 0; i < A.length; i++) {
            superstring(A, i, visited, new StringBuffer());
        }
        return minStr;
    }

    private void superstring(String[] A, int idx, boolean[] visited, StringBuffer sb) {
        if (minStr != null && sb.length() >= minStr.length()) return;

        visited[idx] = true;
        int len = append(sb, A[idx]);

        boolean flag = true;
        // 试探
        for (int i = 0; i < A.length; i++) {
            if (visited[i]) continue;

            flag = false;
            superstring(A, i, visited, sb);
        }

        if (flag && (minStr == null || minStr.length() > sb.length())) minStr = sb.toString();
        
        // 回溯
        visited[idx] = false;
        sb.setLength(sb.length() - len);
    }

    private int append(StringBuffer sb, String str) {
        String temp = sb.toString();
        int idx = 0, len = 0;
        while (idx < str.length()) {
            if (temp.endsWith(str.substring(0, idx + 1))) len = idx + 1;
            idx++;
        }
        sb.append(str.substring(len));
        return str.length() - len;
    }
}
```

## 性能分析

&emsp;



## 官方解题

&emsp;