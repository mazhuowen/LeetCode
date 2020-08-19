[toc]

There is a list of sorted integers from $1$ to $n$. Starting from left to right, remove the first number and every other number afterward until you reach the end of the list.

Repeat the previous step again, but this time from right to left, remove the right most number and every other number from the remaining numbers.

We keep repeating the steps again, alternating left to right and right to left, until a single number remains.

Find the last number that remains starting with a list of length $n$.



## 题目解读

&emsp;从左往右移除第一个元素及之后每隔一个数字移除一次，直到最左边，然后从右往左移除；重复这个过程，直到只剩最后一个数字，返回该数字。

```java
class Solution {
    public int lastRemaining(int n) {
        
    }
}
```

## 程序设计

* 仔细分析，对于`1,2,3,4,5,6`，从左向右移除后得到`2,4,6`，可对该数列整体除以$2$，得到`1,2,3`；然后从右向左移除后得到`2`。
* 可以推导得`f(n)`从左向右移除剩余$\frac{n}{2}$个，此时需要从右向左移除，以上述例子为例，可表示为`4-3,4-2,4-1`，这样得到递归公式`f(n)=2 * (n/2 + 1 - f(n/2))`。

```java
class Solution {
    public int lastRemaining(int n) {
        if (n < 1) throw new IllegalArgumentException("invalid param");

        if (n == 1) return 1;
        else return 2 * (n / 2 + 1 - lastRemaining(n / 2));
    }
}
```

* 社区迭代思路采用模拟法，即每次记录左侧第一个元素和等差值，如果是从左至右移除，则下一轮最左侧值为原先值加等差值；如果从右至左移除，则只有当数列长度为奇数时，最左侧值为原先值加等差值，否则不变。这样模拟直到最后只剩一个元素。

```java
class Solution {
    public int lastRemaining(int n) {
        if (n < 1) throw new IllegalArgumentException("invalid param");

        // 开始的第一个元素
        int first = 1;
        // 等差数列
        int diff = 1;
        // 数列长度
        int remaining = n;
        
        boolean isLeft = true;
        while (remaining > 1) {
            // 从左边开始或从右边开始长度为奇数，则剩余的元素开头为first加差
            if (isLeft || remaining % 2 == 1) {
                first += diff;
            }
            // 如果从右边开始长度为偶数，则起始数字不变
            remaining /= 2;
            diff *= 2;
            isLeft = !isLeft;
        }
        return first;
    }
}
```

## 性能分析

&emsp;递归时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：3 ms, 在所有 Java 提交中击败了99.65%的用户。

内存消耗：37.7 MB, 在所有 Java 提交中击败了53.95%的用户。

&emsp;迭代时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：4ms，在所有java提交中击败了27.43%的用户。

内存消耗：37.7MB，在所有java提交中击败了60.53%的用户。

## 官方解题

&emsp;暂无，密切关注。