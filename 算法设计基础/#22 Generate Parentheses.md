[toc]

Given $n$ pairs of parentheses, write a function to generate all combinations of well-formed parentheses.



## 题目解读

&emsp;给定$n$对括号，找到所有的括号组合形式。

```java
class Solution {
    public List<String> generateParenthesis(int n) {

    }
}
```

## 程序设计

* 仔细观察发现$n$对括号的组合是$2$和$n - 2$，$3$和$n - 3$……对括号的左右组合，其中特殊的是$n - 1$，将$n - 1$的组合嵌入到单括号形成全新的组合。这样可以使用动态规划记录并层层生成新的组合。

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> res = new LinkedList<>();
        if (n <= 0) return res;
        Set<String>[] dp = new Set[n + 1];
        // 初始化第一层
        dp[1] = new HashSet<>();
        dp[1].add("()");
        for (int i = 2; i <= n; i++) {
            dp[i] = new HashSet<>();
            // 由于重复，只遍历一半
            for (int j = 1; j <= i / 2; j++) {
                for (String s : dp[j]) {
                    for (String t : dp[i - j]) {
                        // 左右组合
                        dp[i].add(new StringBuffer(s).append(t).toString());
                        dp[i].add(new StringBuffer(t).append(s).toString());
                    }
                }
            }
			//  嵌入组合
            for (String s : dp[i - 1]) {
                dp[i].add(new StringBuffer("(").append(s).append(")").toString());
            }
        }
        
        for (String s : dp[n]) {
            res.add(s);
        }
        return res;
    }
}
```

## 性能分析

执行用时：3ms，在所有java提交中击败了27.40%的用户。

内存消耗：40.4MB，在所有java提交中击败了5.04%的用户。

## 官方解题

&emsp;官方根据括号组合的规律，巧妙利用结构特性，使用回溯实现：首先括号序列的一大特性是序列中开括号的数目大于等于闭括号的数目，真个序列开闭括号数目一致。有了这个规律可判断字符串是否达到规定要求，没有则添加开括号；其次判断闭括号是否等于开括号，没有则添加闭括号。

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> res = new LinkedList<>();
        generateParenthesis("", 0, 0, n, res);
        return res;
    }

    private void generateParenthesis(String s, int open, int close, int n, List<String> res ) {
        // 一种组合已经找到
        if (s.length() == 2 * n) {
            res.add(s);
            return;
        }
        if (open < n) {
            generateParenthesis(s + "(", open + 1, close, n,  res);
        }
        if (close < open) {
            generateParenthesis(s + ")", open, close + 1, n, res);
        }
    }
}
```

&emsp;时间复杂度为$O(\frac{4^n}{\sqrt{n}})$，空间复杂度为$O(\frac{4^n}{\sqrt{n}})$。

执行用时：1ms，在所有java提交中击败了98.52%的用户。

内存消耗：40.2MB，在所有java提交中击败了5.04%的用户。

&emsp;官方还提供了一种上述动态规划的回溯方法：

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList();
        if (n == 0) {
            ans.add("");
        } else {
            for (int c = 0; c < n; ++c)
                for (String left: generateParenthesis(c))
                    for (String right: generateParenthesis(n-1-c))
                        ans.add("(" + left + ")" + right);
        }
        return ans;
    }
}
```

&emsp;时间复杂度为$O(\frac{4^n}{\sqrt{n}})$，空间复杂度为$O(\frac{4^n}{\sqrt{n}})$。

执行用时：16ms，在所有java提交中击败了5.24%的用户。

内存消耗：40.6MB，在所有java提交中击败了5.04%的用户。