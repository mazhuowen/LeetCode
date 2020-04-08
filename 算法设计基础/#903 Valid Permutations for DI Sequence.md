[toc]


We are given `S`, a length `n` string of characters from the set `{'D', 'I'}`. (These letters stand for "decreasing" and "increasing".)

A *valid permutation* is a permutation `P[0], P[1], ..., P[n]` of integers `{0, 1, ..., n}`, such that for all `i`:

- If `S[i] == 'D'`, then `P[i] > P[i+1]`, and;
- If `S[i] == 'I'`, then `P[i] < P[i+1]`.

How many valid permutations are there? Since the answer may be large, **return your answer modulo `10^9 + 7`**.



Note:

* $1 \le \text{S.length} \le 200$
* `S` consists only of characters from the set `{'D', 'I'}`.



## 题目解读



```java
class Solution {
    public int numPermsDISequence(String S) {

    }
}
```

## 程序设计

* 参考官方解题思路，可以使用动态规划来求解。`dp(i,j)`表示前`i`个位置符合要求的序列，剩余未选元素小于位置`i`的个数为`j`的序列的数目。初始状态，`dp(0,0)`表示位置0的元素大于剩余所有元素，可知只有一种摆放方法，即最小值放在位置0；`dp(0,j)`表示剩余$n$个值有`j`个小于位置0的元素，只需将第`j+1`小的元素放在位置0，只有一种摆放位置。后续需要序列`S`是增加还是降低来决定。
* 如果是`D`，即降低，则`i`位置的值比`i-1`位置的值小；对于`dp(i,j)`，显然`dp(i-1,k)`中`k`要大于`j`，因为要选一个比位置`i-1`小的数，剩余为`k-1`，必须大于等于`j`，这样我们可以在`dp(i-1,k) k>j`的方案中选择`dp(i,j)`方案。

```java

```



## 性能分析



## 官方解题

