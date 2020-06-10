[toc]

Under a grammar given below, strings can represent a set of lowercase words.  Let's use `R(expr)` to denote the **set** of words the expression represents.

Grammar can best be understood through simple examples:

* Single letters represent a singleton set containing that word.

  * `R("a") = {"a"}`
  * `R("w") = {"w"}`
* When we take a comma delimited list of 2 or more expressions, we take the union of possibilities.
  * `R("{a,b,c}") = {"a","b","c"}`
  * `R("{{a,b},{b,c}}") = {"a","b","c"}` (notice the final set only contains each word at most once)
* When we concatenate two expressions, we take the set of possible concatenations between two words where the first word comes from the first expression and the second word comes from the second expression.
  * `R("{a,b}{c,d}") = {"ac","ad","bc","bd"}`
  * `R("a{b,c}{d,e}f{g,h}") = {"abdfg", "abdfh", "abefg", "abefh", "acdfg", "acdfh", "acefg", "acefh"}`

Formally, the 3 rules for our grammar:

* For every lowercase letter `x`, we have `R(x) = {x}`
* For expressions $e_1, e_2, \dots, e_k$ with $k \ge 2$, we have $R(\{e_1,e_2,\cdots\}) = R(e_1) \cup R(e_2) \cup \cdots$
* For expressions $e_1$ and $e_2$, we have $R(e_1 + e_2) = \{a + b\ for\ (a, b)\ in\ R(e_1) \times R(e_2)\}$, where `+` denotes concatenation, and `×` denotes the cartesian product.

Given an `expression` representing a set of words under the given grammar, return the sorted list of words that the expression represents.



Constraints:

* $1 \le \text{expression.length} \le 60$
* `expression[i]` consists of `'{'`, `'}'`, `','` or lowercase English letters.
* The given `expression` represents a set of words based on the grammar given in the description.



## 题目解读

&emsp;给定表达式，求所有可能的子符串，输出按照字典需排列。

```java
class Solution {
    public List<String> braceExpansionII(String expression) {

    }
}
```

## 程序设计

* 仔细分析匹配串的通用形式为`aaa{...}bbb{...}ccc`，括号中是逗号分隔的多个匹配子串，仍然符合上述规律。
* 令`A=aaa{...}`，`B=bbb{...}ccc`，可先处理`A`，`B`可递归调用细化处理，`A`和`B`的关系是组合关系；对于`A`，首先处理前缀`aaa`，然后处理`{...}`中的并集部分。

```java
class Solution {
    public List<String> braceExpansionII(String expression) {
        if (expression == null) throw new IllegalArgumentException("invalid param");

        char[] exp = expression.toCharArray();
        Set<String> set = braceExpansionII(exp, 0, exp.length - 1);

        List<String> res = new LinkedList<>(set);
        // 输出排序
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
		
        // 切分A
        int level = 0, split;
        for (split = start; split <= end; split++) {
            if (exp[split] == '{') level++;
            else if (exp[split] == '}') level--;

            // 找到第一组{..}
            if (level == 0 && exp[split] == '}') break;
        }

        // 遍历A的{..}括号内的同层元素
        level = 0;
        Set<String> sub = new HashSet<>();
        // 记录前一索引
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

        // B
        Set<String> other = braceExpansionII(exp, split + 1, end);

        // 合并前缀和A、B
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

&emsp;最坏情况为`{{{{...a...}}}}`，时间复杂度为$O(N^2)$，最坏情况为`{a,b,...}...{a,b,...}`，空间复杂度为$O(d^{\frac{N}{d + 2}})$，其中$d$为括号内并列字符数目。

执行用时：12ms，在所有java提交中击败了90.91%的用户。

内存消耗：40.3MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。