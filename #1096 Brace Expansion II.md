[toc]





## 题目解读

&emsp;

```java

```

## 程序设计

* 

```java
class Solution {
    public List<String> braceExpansionII(String expression) {
        if (expression == null) throw new IllegalArgumentException("invalid param");

        char[] exp = expression.toCharArray();
        Set<String> set = braceExpansionII(exp, 0, exp.length - 1);

        List<String> res = new LinkedList<>(set);
        Collections.sort(res);
        return res;
    }

    private Set<String> braceExpansionII(char[] exp, int start, int end) {
        Set<String> res = new HashSet<>();
        String prefix;
        /*
         * 对于普遍形式aaa{..}bbb{..}ccc，先遍历到括号处
         */
        // 遍历到第一个开括号处
        int temp = start;
        while (start <= end && exp[start] != '{') start++;
        // 截取前缀
        prefix = new String(exp, temp, start - temp);
        // 没有括号，为一个字符串
        if (start > end) {
            res.add(prefix);
            return res;
        }

        int level = 0, split;
        for (split = start; split <= end; split++) {
            if (exp[split] == '{') level++;
            else if (exp[split] == '}') level--;

            // 找到第一组{..}
            if (level == 0 && exp[split] == '}') break;
        }

        // 遍历第一组{..}括号内的同层元素
        level = 0;
        Set<String> sub = new HashSet<>();
        int pre = start + 1;
        for (int i = start + 1; i < split; i++) {
            if (exp[i] == '{') level++;
            else if (exp[i] == '}') level--;

            // 找到一个同层元素
            if (level == 0 && exp[i] == ',') {
                // 加入同层集合
                sub.addAll(braceExpansionII(exp, pre, i - 1));
                pre = i + 1;
            }
        }
        sub.addAll(braceExpansionII(exp, pre, split - 1));

        Set<String> other = braceExpansionII(exp, split + 1, end);

        for (String str1 : sub) {
            for (String str2 : other) {
                res.add(concat(prefix, str1, str2));
            }
        }
        return res;
    }

    private String concat(String prefix, String str1, String str2) {
        StringBuffer sb = new StringBuffer();
        if (prefix != null) sb.append(prefix);
        sb.append(str1).append(str2);
        return sb.toString();
    }
}
```

## 性能分析

&emsp;



## 官方解题

&emsp;