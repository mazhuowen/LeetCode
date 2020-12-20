[toc]

On a broken calculator that has a number showing on its display, we can perform two operations:

* **Double**: Multiply the number on the display by 2, or;
* **Decrement**: Subtract 1 from the number on the display.

Initially, the calculator is displaying the number `X`.

Return the minimum number of operations needed to display the number `Y`.

 

**Example 1**:

```
Input: X = 2, Y = 3
Output: 2
Explanation: Use double operation and then decrement operation {2 -> 4 -> 3}.
```

**Example 2**:

```
Input: X = 5, Y = 8
Output: 2
Explanation: Use decrement and then double {5 -> 4 -> 8}.
```

**Example 3**:

```
Input: X = 3, Y = 10
Output: 3
Explanation:  Use double, decrement and double {3 -> 6 -> 5 -> 10}.
```

**Example 4**:

```
Input: X = 1024, Y = 1
Output: 1023
Explanation: Use decrement operations 1023 times.
```



**Note**:

* $1 \le X \le 10^9$
* $1 \le Y \le 10^9$



## 题目解读

&emsp;`X`转换为`Y`的最少步数。

```java
class Solution {
    public int brokenCalc(int X, int Y) {

    }
}
```

## 程序设计

* 可从`Y`逆向倒推。

```java
class Solution {

    public int brokenCalc(int X, int Y) {
        if (X > Y) return X - Y;
        return backTracing(Y, X);
    }

    private int backTracing(int Y, int X) {
        if (Y == X) return 0;
        // X减少直到等于Y
        if (Y < X) return X - Y;
        int div = Y / 2, rem = Y % 2;
        // 如果Y不被整除，则加一，然后整除（反向模拟X乘2减1的步骤）
        int move = rem + 1;
        // 整除后的数（如果有余数则加一）
        return backTracing(div + rem, X) + move;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log_2Y)$，空间复杂度为$O(1)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：35.4 MB, 在所有 Java 提交中击败了56.32%的用户。

## 官方解题

&emsp;官方思路类似，采用循环而非递归。

```java
class Solution {
    public int brokenCalc(int X, int Y) {
        int res = 0;
        while (X < Y) {
            if (Y % 2 == 1) Y++;
            else Y /= 2;
            res++;
        }
        return res + X - Y;
    }
}
```

&emsp;时间复杂度为$O(\log_2Y)$，空间复杂度为$O(1)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：35.2 MB, 在所有 Java 提交中击败了79.31%的用户。