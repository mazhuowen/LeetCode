[toc]

You are given a string `expression` representing a Lisp-like expression to return the integer value of.

The syntax for these expressions is given as follows.

* An expression is either an integer, a let-expression, an add-expression, a mult-expression, or an assigned variable. Expressions always evaluate to a single integer.
* (An integer could be positive or negative.)
* A let-expression takes the form (`let v1 e1 v2 e2 ... vn en expr`), where `let` is always the string `"let"`, then there are $1$ or more pairs of alternating variables and expressions, meaning that the first variable `v1` is assigned the value of the expression `e1`, the second variable `v2` is assigned the value of the expression `e2`, and so on **sequentially**; and then the value of this let-expression is the value of the expression expr.
* An add-expression takes the form (`add e1 e2`) where add is always the string "`add`", there are always two expressions `e1`, `e2`, and this expression evaluates to the addition of the evaluation of `e1` and the evaluation of `e2`.
* A mult-expression takes the form (`mult e1 e2`) where mult is always the string "`mult`", there are always two expressions `e1`, `e2`, and this expression evaluates to the multiplication of the evaluation of `e1` and the evaluation of `e2`.
* For the purposes of this question, we will use a smaller subset of variable names. A variable starts with a lowercase letter, then zero or more lowercase letters or digits. Additionally for your convenience, the names "add", "let", or "mult" are protected and will never be used as variable names.
* Finally, there is the concept of scope. When an expression of a variable name is evaluated, **within the context of that evaluation**, the innermost scope (in terms of parentheses) is checked first for the value of that variable, and then outer scopes are checked sequentially. It is guaranteed that every expression is legal. Please see the examples for more details on scope.



**Note**:

* The given string `expression` is well formatted: There are no leading or trailing spaces, there is only a single space separating different components of the string, and no space between adjacent parentheses. The expression is guaranteed to be legal and evaluate to an integer.
* The length of `expression` is at most 2000. (It is also non-empty, as that would not be a legal expression.)
* The answer and all intermediate calculations of that answer are guaranteed to fit in a 32-bit integer.



## 题目解读

&emsp;计算表达式。

```java
class Solution {
    public int evaluate(String expression) {

    }
}
```

## 程序设计

* 由于表达式套表达式，使用递归解析，主要形式有如下：
  * `let`语句，其后可由若干变量名和变量值组成，最后跟一个表达式；变量名只能是字符串，而变量值则比较灵活，可以是数字、变量，也可以是表达式；最后的计算表达式可以是表示式，也可以是单个变量，甚至是固定数字；
  * `add`及`mult`语句，其后只能是两个操作数，可以是数字、表达式及变量；需注意的是这两个并列的操作数如果存在变量，则必须是使用当前语句的参数值，不能覆盖；如`(let x 2 (add (let x 3 (let x 4 x)) x))`，`add`语句中第二个操作数`x`的值应该是最外层的值$2$，而不能混淆为第一个操作数的值$4$。

```java
class Solution {
    public int evaluate(String expression) {
        char[] seq = expression.trim().toCharArray();
        return evaluate(seq, 0, seq.length - 1, new HashMap<>());
    }

    private int evaluate(char[] expression, int left, int right, Map<String, Integer> params) {
        // let后的单变量表达式
        if (expression[left] != '(' || expression[right] != ')') {
            // 变量或数字
            if (expression[left] == '-' || Character.isDigit(expression[left])) return parseInteger(expression, left)[1];
            else return parseParam(expression, left, params)[1];
        }

        left++;
        right--;
        // let语句
        if (expression[left] == 'l') {
            left += 4;
            // 将所有变量和数字加入字典
            while (expression[left] != '(' && expression[parseParam(expression, left, params)[0] - 1] != ')') {
                left = parseLet(expression, left, params);
            }
            // 计算剩余表达式
            return evaluate(expression, left, right, params);
        }
        // 二元操作语句
        else if (expression[left] == 'a' || expression[left] == 'm') {
            boolean add = expression[left] == 'a';
            left += 4;
            if (!add) left++;
            // 获取变量1
            int[] res;
            // 避免并列两个变量的赋值混淆
            Map<String, Integer> tmp1 = new HashMap<>(params);
            // 数字
            if (expression[left] == '-' || Character.isDigit(expression[left])) res = parseInteger(expression, left);
            // 表达式
            else if (expression[left] == '(') res = parseExp(expression, left, tmp1);
            // 变量
            else res = parseParam(expression, left, tmp1);
            int val1 = res[1];
            left = res[0];

            // 获取变量2
            Map<String, Integer> tmp2 = new HashMap<>(params);
            // 数字
            if (expression[left] == '-' || Character.isDigit(expression[left])) res = parseInteger(expression, left);
            // 表达式
            else if (expression[left] == '(') res = parseExp(expression, left, tmp2);
            // 变量
            else res = parseParam(expression, left, tmp2);
            int val2 = res[1];

            return add ? (val1 + val2) : (val1 * val2);
        }
        // 数字
        else return parseInteger(expression, left)[1];
    }

    // 解析数字
    private int[] parseInteger(char[] expression, int idx) {
        int val = 0;
        // 是否是负数
        int flag = 1;
        if (expression[idx] == '-') {
            flag = -1;
            idx++;
        }

        while (idx < expression.length && Character.isDigit(expression[idx])) val = val * 10 + (int)(expression[idx++] - '0');
        // 返回索引和结果
        return new int[]{++idx, flag * val};
    }

    // 解析变量
    private int[] parseParam(char[] expression, int idx, Map<String, Integer> params) {
        StringBuffer param = new StringBuffer();
        while (idx < expression.length && expression[idx] != ' ' && expression[idx] != ')') param.append(expression[idx++]);
        return new int[]{++idx, params.getOrDefault(param.toString(), -1)};
    }

    // 解析表达式
    private int[] parseExp(char[] expression, int idx, Map<String, Integer> params) {
        // 查找表达式结束位置
        int end = idx + 1, level = 1;
        while (end < expression.length) {
            if (expression[end] == '(') level++;
            else if (expression[end] == ')') level--;
            if (level == 0) break;
            end++;
        }
        return new int[]{end + 2, evaluate(expression, idx, end, params)};
    }

    // 赋值变量
    private int parseLet(char[] expression, int idx, Map<String, Integer> params) {
        // 变量名
        StringBuffer param = new StringBuffer();
        while (idx < expression.length && expression[idx] != ' ') param.append(expression[idx++]);
        // 跳过空格
        idx++;

        int[] res;
        // 数字
        if (expression[idx] == '-' || Character.isDigit(expression[idx])) res = parseInteger(expression, idx);
        // 表达式
        else if (expression[idx] == '(') res = parseExp(expression, idx, params);
        // 变量
        else res = parseParam(expression, idx, params);

        params.put(param.toString(), res[1]);
        return res[0];
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了76.67%的用户。

内存消耗：38.9MB，在所有java提交中击败了29.17%的用户。

## 官方解题

&emsp;官方采用栈保存表达式。