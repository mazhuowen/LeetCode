[toc]

Given a number `s` in their binary representation. Return the number of steps to reduce it to 1 under the following rules:

* If the current number is even, you have to divide it by 2.

* If the current number is odd, you have to add 1 to it.

It's guaranteed that you can always reach to one for all testcases.



Constraints:

* $1 \le \text{s.length} \le 500$
* `s` consists of characters `'0'` or `'1'`
* `s[0] == '1'`



## 题目解读

&emsp;判断传入数字变为1的步数。

```java
class Solution {
    public int numSteps(String s) {
		
    }
}
```

## 程序设计

* 最基本的可以将二进制字符串转化为数字模拟。观察二进制数的规律，如果低位为0则可以除2右移，如果是1则加1并除2右移，低位的操作数可以统计；关键问题在于新的低位是0是1的判断，由于之前低位加了1，当前位的值不一定是字符串中的值，可以使用标识来表示当前位是否加一。

```java
class Solution {
    public int numSteps(String s) {
        if (s == null || s.isEmpty()) throw new IllegalArgumentException("invalid param");

        // 当前位是否加一
        boolean flag = false;
        int count = 0;
        char[] strs = s.toCharArray();
        for (int i = strs.length - 1; i >= 0; i--) {
            char c = strs[i];
            // 低位为0直接除2
            if (c == '0' && !flag || c == '1' && flag) count++;
            // 遇到1
            else {
                // 最高位为1，结束
                if (i == 0) break;
                // 非最高位为1，当前位加一并除2
                count += 2;
                // 后续连续的1位加一
                if (!flag) flag = true;
            }
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.1MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。