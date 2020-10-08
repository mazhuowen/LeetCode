[toc]

Given a string `S` of digits, such as `S = "123456579"`, we can split it into a Fibonacci-like sequence `[123, 456, 579]`.

Formally, a Fibonacci-like sequence is a list F of non-negative integers such that:

* $0 \le \text{F[i]} \le 2^31 - 1$, (that is, each integer fits a 32-bit signed integer type);
* $\text{F.length} \ge 3$;
* and $\text{F[i]} + \text{F[i+1]} = \text{F[i+2]}$ for all $0 \le i < \text{F.length} - 2$.

Also, note that when splitting the string into pieces, each piece must not have extra leading zeroes, except if the piece is the number 0 itself.

Return any Fibonacci-like sequence split from `S`, or return `[]` if it cannot be done.



**Note:**

* $1 \le \text{S.length} \le 200$
* `S` contains only digits.



## 题目解读

&emsp;判断一个字符串是否可划分为斐波那契数列，并返回一种划分方法。

```java
class Solution {
    public List<Integer> splitIntoFibonacci(String S) {

    }
}
```

## 程序设计

* 由于长度较短，采用回溯暴力遍历；需注意除了$0$本身，最高位数字不能为$0$，其次在数字转换中需要注意数值溢出的问题；
* 对遍历过程进行剪枝，由于数字是正整数，除了起始的两个数，其余数字呈递增；其次在尝试当前数值`tmp`时，如果`num1 + num2 < tmp`或`tmp`非法，则无需继续遍历数字，终止本轮尝试。

```java
class Solution {
    List<Integer> res;

    public List<Integer> splitIntoFibonacci(String S) {
        res = new LinkedList<>();
        if (S == null || S.isEmpty()) return res;
        backTracing(0, S.toCharArray(), -1, -1, new LinkedList<>());
        return res;
    }

    private boolean backTracing(int idx, char[] strs, int num1, int num2, List<Integer> list) {
        // 剪枝，除了最开始的两个数，其他数都必须递增
        if (list.size() >= 3 && num2 < num1) return false;
        if (idx >= strs.length) {
            // 找到一组可行解
            if (list.size() >= 3) {
                res.addAll(list);
                return true;
            }
            return false;
        }

        for (int i = idx; i < strs.length; i++) {
            int tmp = parseToInt(strs, idx, i);
            // 剪枝，数字不合法，后续数字只会更大无需遍历
            if (tmp == -1 || num1 != -1 && num2 != -1 && tmp - num2 > num1) break;
            // 不等于前两个数之和
            if (num1 != -1 && num2 != -1 && num1 != tmp - num2) continue;

            // 试探
            list.add(tmp);
            if (backTracing(i + 1, strs, num2, tmp, list)) return true;
            // 回溯
            list.remove(list.size() - 1);
        }
        return false;
    }

    private int parseToInt(char[] strs, int s, int e) {
        // 超出整数范围或存在最高位为0
        if (e - s + 1 > 10 || strs[s] == '0' && e - s + 1 >= 2) return -1;
        int num = 0;
        for (int i = s; i < e; i++) num = num * 10 + (strs[i] - '0');
        // 超出整数范围
        if (num > Integer.MAX_VALUE / 10 || num == Integer.MAX_VALUE / 10 && Integer.MAX_VALUE % 10 < (strs[e] - '0')) return -1; 
        return num * 10 + (strs[e] - '0');
    }
}
```

## 性能分析

&emsp;最坏情况下时间复杂度为$O(N^2)$，空间复杂度为$O(N)$

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.4MB，在所有java提交中击败了91.82%的用户。

## 官方解题

&emsp;官方采用暴力循环求解。