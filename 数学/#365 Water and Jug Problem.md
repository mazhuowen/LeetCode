[toc]

You are given two jugs with capacities `x` and `y` litres. There is an infinite amount of water supply available. You need to determine whether it is possible to measure exactly `z` litres using these two jugs.

If `z` liters of water is measurable, you must have `z` liters of water contained within **one or both buckets** by the end.

Operations allowed:

* Fill any of the jugs completely with water.
* Empty any of the jugs.
* Pour water from one jug into another till the other jug is completely full or the first jug itself is empty.



## 题目解读

&emsp;判断是否可通过两个水壶得到指定的水。

```java
class Solution {
    public boolean canMeasureWater(int x, int y, int z) {

    }
}
```

## 程序设计

* 如果$z = x + y$，只需将两个桶装满；如果$z < x + y$，则尝试将较大的桶倒入较小的桶后剩余的水与两个桶的组合$y - x + y$，尝试较少的桶填满较大桶后剩余的水的组合$ax - y + x$，以此类推，上述操作都可以表示为$ax + by = z$。

  * 若$a\ge 0$，$b \ge 0$，那么显然可以达成目标；
  * 若$a \le 0$，那么可以进行以下操作：往$y$壶倒水；把$y$壶的水倒入$x$壶；如果$y$壶不为空，那么$x$壶肯定是满的，把$x$壶倒空，然后再把$y$壶的水倒入$x$壶。重复以上操作直至某一步时**$x$壶进行了$a$次倒空操作，$y$壶进行了$b$次倒水操作**。

  * 若$b \le 0$，方法同上，$x$与$y$互换。

* 根据贝祖定理，$ax + by = z$有解当且仅当$z$是$x, y$的最大公约数的倍数。因此只需要找到最大公约数并判断$z$是否是它的倍数。

```java
class Solution {
    public boolean canMeasureWater(int x, int y, int z) {
        if (x < 0 || y < 0 || z < 0) throw new IllegalArgumentException("invalid param");

        // 特殊情况判断
        if (x + y < z) return false;
        if (x == 0 || y == 0) return z == 0 || z == x + y;

        int gcd = gcd(x, y);
        return z % gcd == 0;
    }

    private int gcd(int x, int y) {
        if (x == 0) return y;
        return gcd(y % x, x);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(\log(\min(x,y)))$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.2MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;官方除了数学思路解决，还提供了图的遍历的方式。如果不考虑过程，可以只考虑水的总量变化。

```java
class Solution {
    public boolean canMeasureWater(int x, int y, int z) {
        if (z < 0 || z > x + y) return false;
        
        Set<Integer> set = new HashSet<>();
        Queue<Integer> q = new LinkedList<>();
        q.offer(0);
        while (!q.isEmpty()) {
            int n = q.poll();
            // 加满x（注意加入集合判断当前水量水否存在死循环）
            if (n + x <= x + y && set.add(n + x)) {
                q.offer(n + x);
            }
            // 加满y
            if (n + y <= x + y && set.add(n + y)) {
                q.offer(n + y);
            }
            // 排空x
            if (n - x >= 0 && set.add(n - x)) {
                q.offer(n - x);
            }
            // 排空y
            if (n - y >= 0 && set.add(n - y)) {
                q.offer(n - y);
            }
            if (set.contains(z)) return true;
        }
        return false;
    }
}
```

&emsp;时间复杂度为$O(xy)$，空间复杂度为$O(xy)$。

执行用时：77ms，在所有java提交中击败了31.13%的用户。

内存消耗：59MB，在所有java提交中击败了50.00%的用户。