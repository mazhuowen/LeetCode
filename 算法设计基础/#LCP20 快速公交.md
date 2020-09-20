[toc]

小扣打算去秋日市集，由于游客较多，小扣的移动速度受到了人流影响：

* 小扣从 `x` 号站点移动至 `x + 1` 号站点需要花费的时间为 `inc`；
* 小扣从 `x` 号站点移动至 `x - 1` 号站点需要花费的时间为 `dec`。

现有 $m$ 辆公交车，编号为 $0$ 到 $m-1$。小扣也可以通过搭乘编号为 $i$ 的公交车，从 $x$ 号站点移动至 $\text{jump[i]}*x$ 号站点，耗时仅为 $\text{cost[i]}$。小扣可以搭乘任意编号的公交车且搭乘公交次数不限。

假定小扣起始站点记作 $0$，秋日市集站点记作 $\text{target}$，请返回小扣抵达秋日市集最少需要花费多少时间。由于数字较大，最终答案需要对 $1000000007$ ($1e9 + 7$) 取模。

注意：小扣可在移动过程中到达编号大于 $\text{target}$ 的站点。



**提示**：

* $1 \le \text{target} \le 10^9$
* $1 \le \text{jump.length, cost.length} \le 10$
* $2 \le \text{jump[i]} \le 10^6$
* $1 \le \text{inc, dec, cost[i]} \le 10^6$



## 题目解读

&emsp;有三种方法移动到其他公交站，求到达目标地的路径数目。

```java
class Solution {
    public int busRapidTransit(int target, int inc, int dec, int[] jump, int[] cost) {

    }
}
```

## 程序设计

* 需借鉴[#1553 Minimum Number of Days to Eat N Oranges](./#1553 Minimum Number of Days to Eat N Oranges.md)的思路，逆向模拟，假设当前为`jump`，则前一步是`target / jump`，此时如有剩余，则需先前进`target % jump`步，然后再跳步；另一种是前一步为`target / jump + 1`，次数少`jump - target % jump`，即先从当前位置后退然后再跳步。

```java
class Solution {
    private static final int MOD = 1_000_000_007;
    int inc, dec;
    int[] jump, cost;
    Map<Long, Long> record;

    public int busRapidTransit(int target, int inc, int dec, int[] jump, int[] cost) {
        this.inc = inc;
        this.dec = dec;
        this.jump = jump;
        this.cost = cost;
        this.record = new HashMap<>();

        return (int)(backTracing(target) % MOD);
    }

    private long backTracing(long target) {
        if (target == 0) return 0;
        if (target == 1) return inc;
        if (record.containsKey(target)) return record.get(target);
        
        long res = target * inc;
        // 此处一定要先设定，避免target / jump + 1还是target，从而导致死循环，此处设置后不会重复执行
        record.put(target, res);
        for (int i = 0 ; i < jump.length; i++) {
            long div = target / jump[i], remained = target % jump[i];
            // 前进一段后跳步
            res = Math.min(res, backTracing(div) + cost[i] + remained * inc);
            // 回退一段后跳步
            res = Math.min(res, backTracing(div + 1) + cost[i] + (jump[i] - remained) * dec);
        }
        record.put(target, res);
        return res;
    }
}
```

## 性能分析

执行用时：20ms，在所有java提交中击败了63.51%的用户。

内存消耗：39.6MB，在所有java提交中击败了85.13%的用户。

## 官方解题

&emsp;暂无，密切关注。同理可采用[#1553 Minimum Number of Days to Eat N Oranges](./#1553 Minimum Number of Days to Eat N Oranges.md)中图的思路来做。

```java
class Solution {
    private static final int MOD = 1_000_000_007;

    public int busRapidTransit(int target, int inc, int dec, int[] jump, int[] cost) {
        // 最小堆，数组保存最小代价、位置
        PriorityQueue<long[]> queue = new PriorityQueue<>((a, b) -> a[0] == b[0] ? (a[1] - b[1]) > 0 ? 1 : -1 : (a[0] - b[0]) > 0 ? 1 : -1);
        queue.add(new long[]{0, target});
        Set<Long> visited = new HashSet<>();

        long res = (long)target * inc;
        while (!queue.isEmpty()) {
            long[] cur = queue.poll();
            long time = cur[0], pos = cur[1];
            if (visited.contains(pos)) continue;
            visited.add(pos);

            // 到达
            if (pos == 1) {
                res = time + inc;
                break;
            }

            // 入队前进的情况
            queue.add(new long[]{time + (pos - 1) * (long)inc, 1});
            for (int i = 0; i < jump.length; i++) {
                // 入队跳跃的情况
                queue.add(new long[]{time + cost[i] + pos % jump[i] * inc, pos / jump[i]});
                queue.add(new long[]{time + cost[i] + (jump[i] - pos % jump[i]) * dec, pos / jump[i] + 1});
            }
        }
        return (int)(res % MOD);
    }
}
```

执行用时：10ms，在所有java提交中击败了97.30%的用户。

内存消耗：39.8MB，在所有java提交中击败了60.81%的用户。