[toc]

Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

Note: The input string may contain letters other than the parentheses `(` and `)`.



## 题目解读

&emsp;移除最少的括号，使得字符串括号匹配。

```java
class Solution {
    public List<String> removeInvalidParentheses(String s) {

    }
}
```

## 程序设计

* 首先对于任意一个不匹配括号串都可以划分为前部分闭括号多出，后部分开括号多出的情况。首先受用栈来找出这个划分点，然后尝试删除多余的括号。
* 在得到划分点和每个区段待删除括号数目后，回溯尝试删除。

```java
class Solution {

    public List<String> removeInvalidParentheses(String s) {
        List<String> res = new LinkedList<>();
        if (s == null) return res;

        // 查找分隔点
        char[] str = s.toCharArray();
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < str.length; i++) {
            char c = str[i];
            if (c == '(') {
                stack.push(i);
            } else if (c == ')') {
                if (!stack.isEmpty() && str[stack.peek()] == '(') stack.pop();
                else stack.push(i);
            }
        }
        // 本身为合法括号对
        if (stack.isEmpty()) {
            res.add(s);
            return res;
        }
        // 统计分割区块和各区块需要删除括号数目
        int idx = -1;
        int num = stack.size();
        int openNum = num, closeNum = 0;
        while (!stack.isEmpty()) {
            if (str[stack.peek()] == ')') {
                openNum = num - stack.size();
                closeNum = stack.size();
                idx = stack.peek();
                break;
            }
            stack.pop();
        }

        // 分别删除括号
        Set<String> preSet = new HashSet<>();
        Set<String> nextSet = new HashSet<>();
        remove(s.substring(0, idx + 1).toCharArray(), 0, closeNum, ')', preSet);
        remove(s.substring(idx + 1).toCharArray(), 0, openNum, '(', nextSet);

        // 前后拼接
        if (preSet.isEmpty()) preSet.add("");
        if (nextSet.isEmpty()) nextSet.add("");
        Set<String> temp = new HashSet<>();
        for (String p : preSet) {
            for (String q : nextSet) {
                temp.add(p + q);
            }
        }
        return new LinkedList<>(temp);
    }

    private void remove(char[] source, int start, int num, char target, Set<String> set) {
        // 终止条件
        if (num == 0) {
            // 需要检验删除后时候符合要求
            if (isValid(source)) set.add(new String(source).replace(" ", ""));
            return;
        }
        if (start >= source.length) return;

        for (int i = start; i <= source.length - num; i++) {
            if (source[i] != target) continue;
            // 试探
            source[i] = ' ';
            remove(source, i + 1, num - 1, target, set);
            // 回溯
            source[i] = target;
        }
    }

    private boolean isValid(char[] str) {
        int count = 0;
        for (char c : str) {
            if (c == ')') count--;
            else if (c == '(') count++;
            // 序列中闭括号不能多于开括号
            if (count < 0) return false;
        }
        return count == 0;
    }
}
```

* 不使用栈，直接遍历计数判断：

```java
class Solution {

    public List<String> removeInvalidParentheses(String s) {
        List<String> res = new LinkedList<>();

        // 查找分隔点
        char[] str = s.toCharArray();

        // 统计分割区块和各区块需要删除括号数目
        int idx = -1;
        int count = 0, openNum = 0, closeNum = 0;
        for (int i = 0; i < str.length; i++) {
            char c = str[i];
            if (c == '(') count++;
            else if (c == ')') count--;

            // 说明当前闭括号多于开括号，需要删除
            if (count < 0) {
                idx = i;
                closeNum -= count;
                // 重置（关键）
                count = 0;
            }
        }
        // 最后多余的开括号数
        if (count > 0) openNum = count;

        // 本身为合法括号对
        if (closeNum == 0 && openNum == 0) {
            res.add(s);
            return res;
        }

        // 分别删除前面的闭括号和后面的开括号
        Set<String> preSet = new HashSet<>(), nextSet = new HashSet<>();
        remove(s.substring(0, idx + 1).toCharArray(), 0, closeNum, ')', preSet);
        remove(s.substring(idx + 1).toCharArray(), 0, openNum, '(', nextSet);

        // 前后拼接，为空则添加空，避免不遍历的情况
        if (preSet.isEmpty()) preSet.add("");
        if (nextSet.isEmpty()) nextSet.add("");
        // 排除重复
        Set<String> temp = new HashSet<>();
        for (String p : preSet) {
            for (String q : nextSet) {
                temp.add(p + q);
            }
        }
        return new LinkedList<>(temp);
    }

    private void remove(char[] source, int start, int num, char target, Set<String> set) {
        // 终止条件
        if (num == 0) {
            if (isValid(source, 0, source.length - 1)) set.add(new String(source).replace(" ", ""));
            return;
        }
        if (start >= source.length) return;

        for (int i = start; i <= source.length - num; i++) {
            if (source[i] != target) continue;
            // 试探
            source[i] = ' ';
            if (isValid(source, 0, i)) remove(source, i + 1, num - 1, target, set);
            // 回溯
            source[i] = target;
        }
    }

    private boolean isValid(char[] str, int i, int j) {
        int count = 0;
        for (; i <= j; i++) {
            char c = str[i];
            if (c == ')') count--;
            else if (c == '(') count++;
            // 序列中闭括号不能多于开括号
            if (count < 0) return false;
        }

        return true;
    }
}
```

## 性能分析

&emsp;原始算法性能如下：

执行用时：6ms，在所有java提交中击败了77.07%的用户。

内存消耗：40MB，在所有java提交中击败了66.67%的用户。

&emsp;改进算法性能如下：

执行用时：5ms，在所有java提交中击败了82.12%的用户。

内存消耗：40.3MB，在所有java提交中击败了66.67%的用户。

## 官方解题

&emsp;官方思路同上，实现不同，在统计得到所有删除括号后，整体进行尝试回溯，不分开。