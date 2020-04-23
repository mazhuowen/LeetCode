[toc]

A frog is crossing a river. The river is divided into x units and at each unit there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.

Given a list of stones' positions (in units) in sorted ascending order, determine if the frog is able to cross the river by landing on the last stone. Initially, the frog is on the first stone and assume the first jump must be 1 unit.

If the frog's last jump was $k$ units, then its next jump must be either $k - 1$, $k$, or $k + 1$ units. Note that the frog can only jump in the forward direction.



Note:

* The number of stones is $\ge 2$ and is $< 1,100$.
* Each stone's position will be a non-negative integer $< 2^{31}$.
* The first stone's position is always $0$.



## 题目解读

&emsp;判断青蛙是否可到达最后一个石头，每次青蛙的跳跃距离是上一次距离加一减一或不变。初始跳跃长度为1。

```java
class Solution {
    public boolean canCross(int[] stones) {
        
    }
}
```

## 程序设计

* 首先想到的是回溯，时间复杂度为$O(3^N)$，会超时。

```java
class Solution {
    public boolean canCross(int[] stones) {
        if (stones == null || stones.length < 2) throw new IllegalArgumentException("invalid param");
		// 判断初始是否满足条件
        if (stones[1] - stones[0] != 1) return false;
        return canCross(stones, 1, 1);
    }

    // idx当前石头，jump上一次跳跃距离
    private boolean canCross(int[] stones, int idx, int jump) {
        // 到达最后石头
        if (idx == stones.length - 1) return true;
		
        // 当前可到达的最大距离
        int max = jump + 1 + stones[idx];
        for (int i = idx + 1; i < stones.length && stones[i] <= max; i++) {
            // 可跳跃到达
            if (stones[i] == max || stones[i] == max - 1 || stones[i] == max - 2) {
                if (canCross(stones, i, stones[i] - stones[idx])) return true;
            }
        }
        return false;
    }
}
```

* 上述回溯存在重复判断的问题，引入额外数据结构存储计算信息。

```java
class Solution {
    // 记录中间结果
    Map<String, Boolean> record;

    public boolean canCross(int[] stones) {
        if (stones == null || stones.length < 2) throw new IllegalArgumentException("invalid param");

        if (stones[1] - stones[0] != 1) return false;

        record = new HashMap<>();
        return canCross(stones, 1, 1);
    }

    // idx当前石头，jump上一次跳跃距离
    private boolean canCross(int[] stones, int idx, int jump) {
        // 到达最后石头
        if (idx == stones.length - 1) return true;
        String key = idx + "-" + jump;
        if (record.get(key) != null) return record.get(key);

        // 当前可到达的最大距离
        int max = jump + 1 + stones[idx];
        for (int i = idx + 1; i < stones.length && stones[i] <= max; i++) {
            // 可跳跃到达
            if (stones[i] == max || stones[i] == max - 1 || stones[i] == max - 2) {
                if (canCross(stones, i, stones[i] - stones[idx])) {
                    record.put(key, true);
                    return true;
                }
            }
        }
        record.put(key, false);
        return false;
    }
}
```

## 性能分析

&emsp;&emsp;时间复杂度为$O(N^3)$，空间复杂度为$O(N^2)$。

执行用时：32ms，在所有java提交中击败了79.34%的用户。

内存消耗：44.4MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供了动态规划的思路，使用哈希表保存前一步的跳跃集合，从前一步再迭代到下一步。

```java
class Solution {

    public boolean canCross(int[] stones) {
        if (stones == null || stones.length < 2) throw new IllegalArgumentException("invalid param");

        if (stones[1] - stones[0] != 1) return false;

        // 动态字典，保存到达当前石头前的跳跃步数
        Map<Integer, Set<Integer>> dp = new HashMap<>();
        for (int stone : stones) {
            dp.put(stone, new HashSet<>());
        }
        // 初始化起点
        dp.get(0).add(0);

        for (int stone : stones) {
            for (int jump : dp.get(stone)) {
                for (int j = -1; j <= 1; j++) {
                    int newJump = jump + j;
                    // 当前位置可达石头
                    if (newJump > 0 && dp.containsKey(stone + newJump)) {
                        dp.get(stone + newJump).add(newJump);
                    }
                }
            }
        }
        return dp.get(stones[stones.length - 1]).size() > 0;
    }
}
```

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$。

执行用时：53ms，在所有java提交中击败了62.90%的用户。

内存消耗：44.4MB，在所有java提交中击败了100.00%的用户。