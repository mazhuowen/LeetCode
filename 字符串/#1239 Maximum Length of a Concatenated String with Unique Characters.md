[toc]



```java
class Solution {
    int count;

    public int maxLength(List<String> arr) {
        // 标记单词组合所占字符
        boolean[] visited = new boolean[26];
        Set<String> set = new HashSet<>(arr);

        maxLength(new StringBuffer(), set, visited);
        return count;
    }

    private void maxLength(StringBuffer sb, Set<String> set, boolean[] visited) {
        int len = sb.length();
        count = Math.max(count, len);

        // 尝试
        Set<String> temp = new HashSet<>(set);

        for (String t : temp) {
            // 字符有冲突
            if (!isValid(t, visited)) continue;

            set.remove(t);
            sb.append(t);
            maxLength(sb, set, visited);

            // 回溯
            set.add(t);
            sb.setLength(len);
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

