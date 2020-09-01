[toc]

A magical string `S` consists of only '1' and '2' and obeys the following rules:

The string `S` is magical because concatenating the number of contiguous occurrences of characters '1' and '2' generates the string `S` itself.

The first few elements of string `S` is the following: `S = "1221121221221121122……"`

If we group the consecutive '1's and '2's in `S`, it will be:

```
1 22 11 2 1 22 1 22 11 2 11 22 ......
```

and the occurrences of '1's or '2's in each group are:

```
1 2 2 1 1 2 1 2 2 1 2 2 ......
```

You can see that the occurrence sequence above is the `S` itself.

Given an integer $N$ as input, return the number of '1's in the first $N$ number in the magical string `S`.



**Note**: $N$ will not exceed $100000$.



## 题目解读

&emsp;定义神奇字符串为只包含`1`和`2`，且对该字符串中元素的计数与该字符串相等。给定$N$，求前$N$个字符串中`1`的数目。

```java
class Solution {
    public int magicalString(int n) {

    }
}
```

## 程序设计

* 显然神奇字符串的计数字符串短于神奇字符串且是神奇字符串的一部分，这样可以逆向思维，从计数字符串不断生成神奇字符串。
* 以`1`为例，表示当前数字为`1`或`2`，其后必然是`2`或`1`，否则计数大于$1$；以`2`为例，表示当前数字必然是`11`或`22`，其后必然是`2`或`1`。
* 题目中神奇字符串以`1`开头。

```java
class Solution {
    public int magicalString(int n) {
        if (n < 1) return 0;

        int[] seqs = new int[n + 1];
        // 初始化位置0为1
        seqs[0] = 1;
        // i位置生成idx+1位置的数字，1的数目
        int idx = 0, count = 1;
        for (int i = 0; idx < n - 1; i++) {
            // 当前序列是1，则idx+1位的数字不能和idx相同
            if (seqs[i] == 1) {
                if (seqs[idx++] == 1) seqs[idx] = 2;
                else {
                    count++;
                    seqs[idx] = 1;
                }
            } 
            // 当前序列为2，则idx+2位的数字不能和idx相同
            else {
                // idx与idx+1位置数字相等（idx位置在seqs[i]=1的情况下已更新）
                seqs[idx + 1] = seqs[idx];
                // 更新idx+2位置的数字
                if (seqs[++idx] == 2) {
                    seqs[++idx] = 1;
                } else {
                    seqs[++idx] = 2;
                }
                count++;
            }
        }
        // 返回结果
        return count - (seqs[n] == 1 ? 1 : 0);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：4ms，在所有java提交中击败了86.52%的用户。

内存消耗：38.8MB，在所有java提交中击败了48.15%的用户。

## 官方解题

&emsp;暂无，密切关注。