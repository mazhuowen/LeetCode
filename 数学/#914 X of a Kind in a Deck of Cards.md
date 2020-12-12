[toc]

In a deck of cards, each card has an integer written on it.

Return `true` if and only if you can choose $X \ge 2$ such that it is possible to split the entire deck into 1 or more groups of cards, where:

* Each group has exactly $X$ cards.
* All the cards in each group have the same integer.



**Example 1**:

```
Input: deck = [1,2,3,4,4,3,2,1]
Output: true
Explanation: Possible partition [1,1],[2,2],[3,3],[4,4].
```

**Example 2**:

```
Input: deck = [1,1,1,2,2,2,3,3]
Output: false´
Explanation: No possible partition.
```

**Example 3**:

```
Input: deck = [1]
Output: false
Explanation: No possible partition.
```

**Example 4**:

```
Input: deck = [1,1]
Output: true
Explanation: Possible partition [1,1].
```

**Example 5**:

```
Input: deck = [1,1,2,2,2,2]
Output: true
Explanation: Possible partition [1,1],[2,2],[2,2].
```



**Constraints**:

* $1 \le \text{deck.length} \le 10^4$
* $0 \le \text{deck[i]} < 10^4$



## 题目解读

&emsp;判断是否可将数组切分为长度大于等于$2$的子集，每个子集数字相等。

```java
class Solution {
    public boolean hasGroupsSizeX(int[] deck) {

    }
}
```

## 程序设计

* 统计每个数字的数目，可切分的最大程度就是所有的数目的最大公约数，判断公约数是否大于等于$2$。

```java
class Solution {
    public boolean hasGroupsSizeX(int[] deck) {
        if (deck == null || deck.length < 2) return false;
        Arrays.sort(deck);
        if (deck[0] == deck[deck.length - 1]) return true;

        int idx = 0, gcd = 0;
        while (idx < deck.length) {
            // 当前数字计数
            int count = 0;
            while (idx < deck.length && deck[idx] == deck[idx - count]) {
                idx++;
                count++;
            }
            gcd = gcd(gcd, count);
            // 无法切分为长度大于1的子段
            if (gcd < 2) return false;
        }
        return true;
    }

    private int gcd(int x, int y) {
        if (x == 0) return y;
        return gcd(y % x, x);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(1)$。

执行用时：3 ms, 在所有 Java 提交中击败了80.72%的用户

内存消耗：39.2 MB, 在所有 Java 提交中击败了38.35%的用户。

## 官方解题

&emsp;官方思路类似，计数采用空间换取时间，哈希表计数。
