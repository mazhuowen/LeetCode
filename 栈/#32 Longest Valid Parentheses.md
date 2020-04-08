[toc]

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.



## 题目解读

&emsp;给定字符串只包含开闭括号，给出有效的最长括号子序列。

```java
class Solution {
    public int longestValidParentheses(String s) {

    }
}
```

## 程序设计

* 首先想到的是暴力遍历，每次从前位置出发截取其后的字符串，使用栈来匹配判断这个子串是否是有效的，根据判断更新长度。时间复杂度为$O(N^2)$，会超时。

```java
class Solution {
    public int longestValidParentheses(String s) {
        if (s == null || s.length() < 2) return 0;

        int max = 0;
        char[] chars = s.toCharArray();
        for (int i = 0; i < chars.length; i++) {
            // 遍历判断ｉ后的字符串是否是连续有效的括号对
            for (int j = i + 1; j < chars.length; j += 2) {
                if (isValid(chars, i, j)) {
                    max = Math.max(max, j - i + 1);
                }
            }
        }
        return max;
    }

    private boolean isValid(char[] chars, int start, int end) {
        Stack<Character> stack = new Stack<>();
        for (int i = start; i <= end; i++) {
            // 开括号入栈
            if (chars[i] == '(') stack.push('(');
            // 闭括号则出栈开括号
            else if (!stack.isEmpty() && stack.peek() == '(') stack.pop();
            // 当前为闭括号，但栈为空或栈顶不是开括号
            else return false;
        }
        // 栈中剩余开括号
        return stack.isEmpty();
    }
}
```

* 官方还提供了动态规划的思路，每个闭括号的位置保存以其结尾的连续括号序列长度。这样如果当前位置i与其前构成一对括号对，则`dp[i] = dp[i-2]+2`；如果当前和其前构不成括号对，如都是闭括号，则判断是否是嵌套括号对，如`()(())`，当前位置为5，前面位置的连续括号序列为2，则需要检查`5-2-1=2`位置是否是开括号，如果是则构成嵌套括号对，然后还需判断2的位置前是否存在有效括号序列，存在还需连起来，即`dp[i] = dp[i - 1]+2+dp[i-dp[i-1]-2]`。

```java
class Solution {
    public int longestValidParentheses(String s) {
        if (s == null || s.length() < 2) return 0;

        int max = 0, len = s.length();
        // 动态规划，保存到当前闭括号之前最大连续长度
        int[] dp = new int[len];
        for (int i = 1; i < len; i++) {
            char c = s.charAt(i);
            // 开括号则继续
            if (c == '(') continue;
            // 当前闭括号，之前为开括号，组成有效括号对
            if (s.charAt(i - 1) == '(') {
                // 如果i-2处是开括号或无效的闭括号，则ｄp[i-2]为０；如果i-2是连续有效的括号，则更新
                dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2;
            }
            // 当前闭括号，之前为闭括号。检查是否是嵌套括号对
            else if (i - 1 - dp[i - 1] >= 0 && s.charAt(i - 1 - dp[i - 1]) == '(') {
                // i位置前存在连续括号对，假设长度为n，则判断i-n-1位置是否是开括号，
                // 如果是则i到i-n-1之间构成连续括号，其次还要加上i-n-1之前的连续括号数，即i-n-2结尾的括号对数
                dp[i] = dp[i - 1] + 2 + (i - 2 - dp[i - 1] >= 0 ? dp[i - 2 - dp[i - 1]] : 0);
            }
            max = Math.max(max, dp[i]);
        }
        return max;
    }
}
```

* 实际上括号问题可用栈快速形象的解决。栈中保存字符在字符串中的索引，每次匹配出栈，都通过索引得到长度并更新。

```java
class Solution {
    public int longestValidParentheses(String s) {
        if (s == null || s.length() < 2) return 0;

        int max = 0, len = s.length();
        char[] chars = s.toCharArray();
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < len; i++) {
            char c = chars[i];
            // 组成括号对
            if (c == ')' && !stack.isEmpty() && chars[stack.peek()] == '(') {
                // 弹出开括号
                stack.pop();
                // 计算序列长度，注意栈为空的情况
                max = Math.max(max, i - (stack.isEmpty() ? -1 : stack.peek()));
            } else {
                // 开括号或不满足条件的闭括号，入栈索引
                stack.push(i);
            }
        }
        return max;
    }
}
```

## 性能分析

&emsp;动态规划时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.4MB，在所有java提交中击败了5.65%的用户。

&emsp;栈时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了48.65%的用户。

内存消耗：39.3MB，在所有java提交中击败了5.65%的用户。

## 官方解题

&emsp;除了上述思路，官方还提供了一种思路。有效括号对有个重要特性就是从左往右遍历，开括号数大于等于闭括号数，因为闭括号数大于开括号数，没法在后面加开括号消去这个闭括号。这样我们只需记录开闭括号数并比较，如果相等则记录更新序列长度，如果开括号数小于闭括号数，表示之前的序列不合法，计数归零重新计数。上述流程可以很好的检测闭括号大于开括号的情况，但无法检测开括号大于闭括号的情况，只需反向遍历，此时有效括号对闭括号数一定大于等于开括号数。

```java
class Solution {
    public int longestValidParentheses(String s) {
        if (s == null || s.length() < 2) return 0;

        int max = 0, len = s.length();
        char[] chars = s.toCharArray();
        // 开闭括号计数
        int close = 0, open = 0;

        // 判断闭括号
        for (int i = 0; i < len; i++) {
            if (chars[i] == '(') open++;
            else close++;

            if (close == open) max = Math.max(max, 2 * close);
            if (close > open) close = open = 0;
        }
		// 反向判断开括号
        close = open = 0;
        for (int i = len - 1; i >= 0; i--) {
            if (chars[i] == '(') open++;
            else close++;

            if (close == open) max = Math.max(max, 2 * close);
            if (close < open) close = open = 0;
        }

        return max;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了92.50%的用户。

内存消耗：39.3MB，在所有java提交中击败了5.65%的用户。