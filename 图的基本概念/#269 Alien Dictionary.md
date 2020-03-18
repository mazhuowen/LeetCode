[toc]

There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of **non-empty** words from the dictionary, where **words are sorted lexicographically by the rules of this new language**. Derive the order of letters in this language.

**Note:**

* You may assume all letters are in lowercase.
* You may assume that if a is a prefix of b, then a must appear before b in the given dictionary.
* If the order is invalid, return an empty string.
* There may be multiple valid order of letters, return any one of them is fine.



## 题目解读

&emsp;实际就是拓扑排序。

```java
class Solution {
    public String alienOrder(String[] words) {

    }
}
```

## 程序设计



```java
class Solution {
    public String alienOrder(String[] words) {
        // 无效情况判断
        if (words == null || words.length == 0) return "";
        // 特殊情况，只有一个字符串，没有边，没有依赖关系，直接返回
        if (words.length == 1) return words[0];
        // 标记存在的字母，记录结点集
        boolean[] flag = new boolean[26];
        for (String word : words) {
            for (char c : word.toCharArray())
                flag[c - 'a'] = true;
        }

        // 图，记录边集
        int[][] graph = new int[26][26];
        // 入度
        int[] degree = new int[26];
        for (int i = 0; i < words.length - 1; i++) {
            String pre = words[i];
            String next = words[i + 1];
            int len = Math.min(pre.length(), next.length());
            for (int j = 0; j < len; j++) {
                if (pre.charAt(j) != next.charAt(j)) {
                    // 存在重复边，避免记录多次，造成入度有误
                    if (graph[pre.charAt(j) - 'a'][next.charAt(j) - 'a'] == 0) {
                        graph[pre.charAt(j) - 'a'][next.charAt(j) - 'a'] = 1;
                        degree[next.charAt(j) - 'a']++;
                    }
                    // 遇到第一个不相等的字符，找到边，后续不再遍历
                    break;
                }
            }
        }
        // 初始化队列
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < 26; i++) {
            // 存在字母且入度为0
            if (flag[i] && degree[i] == 0) queue.add(i);
        }
        // 拓扑排序，拼接
        StringBuffer sb = new StringBuffer();
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            sb.append((char) (cur + 'a'));
            // 结点标记为无
            flag[cur] = false;
            for (int i = 0; i < 26; i++) {
                // 遍历邻接点，加入符合要求的点
                if (flag[i] && graph[cur][i] == 1) {
                    if (--degree[i] == 0) queue.add(i);
                }
            }
        }
        // 判断环，如果还存在入度大于0的结点，表示有环
        for (int i = 0; i < 26; i++) {
            if (degree[i] > 0) return "";
        }
        // 遍历孤点，没有任何依赖关系，随意加入
        for (int i = 0; i < 26; i++) {
            if (flag[i]) sb.append((char) (i + 'a'));
        }
        return sb.toString();
    }
}
```



## 性能分析



## 官方解题