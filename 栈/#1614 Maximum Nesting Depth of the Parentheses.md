[toc]

A string is a **valid parentheses string** (denoted **VPS**) if it meets one of the following:

* It is an empty string `""`, or a single character not equal to `"("` or `")"`,
* It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are **VPS**'s, or
* It can be written as (`A`), where `A` is a **VPS**.

We can similarly define the **nesting depth** `depth(S)` of any VPS `S` as follows:

* `depth("") = 0`
* `depth(A + B) = max(depth(A), depth(B))`, where `A` and `B` are VPS's
* `depth("(" + A + ")") = 1 + depth(A)`, where `A` is a VPS.

For example, `""`, `"()()"`, and `"()(()())"` are **VPS**'s (with nesting depths 0, 1, and 2), and `")("` and `"(()"` are not **VPS**'s.

Given a **VPS** represented as string `s`, return the **nesting depth** of `s`.

 

**Constraints**:

* $1 \le \text{s.length} \le 100$
* `s` consists of digits 0-9 and characters `'+'`, `'-'`, `'*'`, `'/'`, `'('`, and `')'`.
* It is guaranteed that parentheses expression `s` is a **VPS**.



## 题目解读

&emsp;统计括号的最大深度。

```java
class Solution {
    public int maxDepth(String s) {
        
    }
}
```

## 程序设计

* 使用层级来记录当前栈的深度即可。

```java
class Solution {
    public int maxDepth(String s) {
        int level = 0, max = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') {
                level++;
                max = Math.max(max, level);
            }
            else if (c == ')') level--;
            
        }
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.7MB，在所有java提交中击败了93.56%的用户。

## 官方解题

&emsp;暂无，密切关注。