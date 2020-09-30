[toc]

A company is planning to interview $2n$ people. Given the array costs where `costs[i] = [aCosti, bCosti]`, the cost of flying the `i`th person to city `a` is `aCosti`, and the cost of flying the `i`th person to city `b` is `bCosti`.

Return the minimum cost to fly every person to a city such that exactly $n$ people arrive in each city.



**Constraints**:

* $2n == \text{costs.length}$
* $2 \le \text{costs.length} \le 100$
* `costs.length` is even.
* $1 \le \text{aCosti, bCosti} \le 1000$



## 题目解读

&emsp;安排一班人到城市`a`，一半到`b`使得代价最小。

```java
class Solution {
    public int twoCitySchedCost(int[][] costs) {

    }
}
```

## 程序设计

* 如果只考虑一个城市的飞机票，根据城市`a`排序然后取前$n$个去城市`a`，后面的去城市`b`，如`[3,100],[4,500]`这样选择的话总的代价为$3 + 500$，显然第一个人去城市`b`更划算，代价为$100 + 4$；
* 根据代价差排序，如上述例子就是`-97,-496`，然后贪婪决定前$n$个去城市`a`，后$n$个去城市`b`。

```java
class Solution {
    public int twoCitySchedCost(int[][] costs) {
        int n = costs.length / 2;
        // 根据价格差排序
        Arrays.sort(costs, (a, b) -> a[0] - a[1] - b[0] + b[1]);
        int res = 0;
        // 前半部分选择城市a后半部分选择城市b
        for (int i = 0; i < n; i++) res += costs[i][0] + costs[i + n][1];
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了58.94%的用户。

内存消耗：36.9MB，在所有java提交中击败了51.27%的用户。

## 官方解题

&emsp;官方思路同上。