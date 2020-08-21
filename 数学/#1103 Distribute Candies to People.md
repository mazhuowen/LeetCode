[toc]

We distribute some number of `candies`, to a row of `n = num_people` people in the following way:

We then give $1$ candy to the first person, $2$ candies to the second person, and so on until we give $n$ candies to the last person.

Then, we go back to the start of the row, giving `n + 1` candies to the first person, `n + 2` candies to the second person, and so on until we give `2 * n` candies to the last person.

This process repeats (with us giving one more candy each time, and moving to the start of the row after we reach the end) until we run out of candies.  The last person will receive all of our remaining candies (not necessarily one more than the previous gift).

Return an array (of length `num_people` and sum `candies`) that represents the final distribution of candies.



**Constraints:**

- $1 \le \text{candies} \le 10^9$
- $1 \le \text{num_people} \le 1000$



## 题目解读

&emsp;给定糖果总数，分配给$n$个人，假设分配轮次为$e$，则第一个人分配$1 + (e - 1) * n$个糖果，其后的人分派糖果数递增，求最后的分配数目。

```java
class Solution {
    public int[] distributeCandies(int candies, int num_people) {
        
    }
}
```

## 程序设计

* 最基本的方法是模拟分派；假设分派完整的轮次为$row$，如果计算出完整轮次，则可计算出每个人分派的糖果数目，最后再模拟最后一轮的糖果分派。
* 第一轮分派糖果数目为$(1 + n) * n / 2$，第二轮每个人比第一轮多$n$，整体多出$n^2$，第三轮又比第二轮多出$n^2$个糖果，用$e$代表$(1 + n) * n / 2$，$d$代表$n^2$，则完整轮次分派的糖果数目为$e + e + d + e + 2*d + \dots + e + (row - 1)*d = \frac{row*(row - 1)}{2}d + ed \le candies$，可得到$row \le \sqrt{\frac{2candies}{d} + (\frac{2e - d}{2d})^2} - \frac{2e - d}{2d}$，这样可计算出完整轮次$row$。
* 得到完整轮次后计算第$i$个人的糖果分派，第$i$个人第一轮分派$i$个糖果，后续等差为$n$的分派糖果，$row$轮分派$i + i + n + \dots + i + (row - 1) * n = row * i + \frac{row*(row - 1)}{2}*n$。
* 最后模拟最后一轮的分派情况。

```java
class Solution {
    public int[] distributeCandies(int candies, int num_people) {
        int[] res = new int[num_people];
        // 等差数列之和
        int epoch = num_people * (num_people + 1) / 2;
        // 每轮次之差
        int diff = num_people * num_people;

        // 完整分派的轮次
        double tmp = (2 * (double)epoch - diff) / (2 * diff);
        int row = (int)(Math.sqrt(2 * (double)candies / diff + tmp * tmp) - tmp);
        // 根据完整轮次计算每个人的数目
        for (int i = 1; i <= num_people; i++) {
            res[i - 1] = (2 * i + (row - 1) * num_people) * row / 2;
            candies -= res[i - 1];
        }

        // 最后一个轮次模拟分派
        for (int i = 1; i <= num_people && candies > 0; i++) {
            int num = Math.min(i + row * num_people, candies);
            res[i - 1] += num;
            candies -= num;
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.5MB，在所有java提交中击败了7.84%的用户。

## 官方解题

&emsp;官方思路类似。