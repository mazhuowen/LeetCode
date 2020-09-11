[toc]

Alice has a `hand` of cards, given as an array of integers.

Now she wants to rearrange the cards into groups so that each group is size $W$, and consists of $W$ consecutive cards.

Return `true` if and only if she can.



**Constraints**:

* $1 \le \text{hand.length} \le 10000$
* $0 \le \text{hand[i]} \le 10^9$
* $1 \le W \le \text{hand.length}$



## 题目解读

&emsp;给定数组和$W$，判断是否数组中的数可以组成若干长度为$W$的连续数组。

```java
class Solution {
    public boolean isNStraightHand(int[] hand, int W) {

    }
}
```

## 程序设计

* 首先排序数组，然后边遍历边计数，如果其前存在$W$个数字，则减去计数。最后判断哈希表是否为空。

```java
class Solution {
    public boolean isNStraightHand(int[] hand, int W) {
        // 排序
        Arrays.sort(hand);
        // 计数
        Map<Integer, Integer> record = new HashMap<>();
        for (int num : hand) {
            // 判断前面是否存在W个数字，存在则减去计数
            if (isExit(num, record, W)) delete(num, record, W);
            else record.put(num, record.getOrDefault(num, 0) + 1);
        }
        return record.isEmpty();
    }

    private boolean isExit(int cur, Map<Integer, Integer> record, int W) {
        for (int i = 1; i < W; i++) {
            if (!record.containsKey(cur - i)) return false;
        }
        return true;
    }

    private void delete(int cur, Map<Integer, Integer> record, int W) {
        for (int i = 1; i < W; i++) {
            if (record.get(cur - i) == 1) record.remove(cur - i);
            else record.put(cur - i, record.get(cur - i) - 1);
        }
    }
}
```

* 修改思路，先计数，再判断。

```java
class Solution {
    public boolean isNStraightHand(int[] hand, int W) {
        Arrays.sort(hand);
        // 计数
        Map<Integer, Integer> record = new HashMap<>();
        for (int num : hand) {
            record.put(num, record.getOrDefault(num, 0) + 1);
        }
        // 判断
        for (int num : hand) {
            int card = record.get(num);
            if (card == 0) continue;
            record.put(num, 0);
            
            for (int i = 1; i < W; i++) {
                if (record.getOrDefault(num + i, 0) < card) return false;
                record.put(num + i, record.get(num + i) - card);
            }
        }
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NW)$，空间复杂度为$O(N)$。

执行用时：1027ms，在所有java提交中击败了5.23%的用户。

内存消耗：41.7MB，在所有java提交中击败了6.20%的用户。

&emsp;优化后时间复杂度为$O(N\frac{N}{W})$，空间复杂度为$O(N)$。

执行用时：27ms，在所有java提交中击败了86.34%的用户。

内存消耗：40.6MB，在所有java提交中击败了73.10%的用户。

## 官方解题

&emsp;官方采用`TreeMap`。