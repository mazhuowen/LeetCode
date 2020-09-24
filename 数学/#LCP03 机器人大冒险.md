[toc]

力扣团队买了一个可编程机器人，机器人初始位置在原点$(0, 0)$。小伙伴事先给机器人输入一串指令`command`，机器人就会**无限循环**这条指令的步骤进行移动。指令有两种：

* `U`: 向y轴正方向移动一格
* `R`: 向x轴正方向移动一格。

不幸的是，在 xy 平面上还有一些障碍物，他们的坐标用`obstacles`表示。机器人一旦碰到障碍物就会被损毁。

给定终点坐标$(x, y)$，返回机器人能否完好地到达终点。如果能，返回`true`；否则返回`false`。



**限制**：

* $2 \le \text{command的长度} \le 1000$
* `command`由`U`，`R`构成，且至少有一个`U`，至少有一个`R`
* $0 \le x \le 1e9$, $0 \le y \le 1e9$
* $0 \le \text{obstacles的长度} \le 1000$
* `obstacles[i]`不为原点或者终点



## 题目解读

&emsp;给定机器人和移动指令，判断是否可安全到达终点。

```java
class Solution {
    public boolean robot(String command, int[][] obstacles, int x, int y) {

    }
}
```

## 程序设计

* 由于机器人无线循环，且机器人只能向上或向右移动，则可模拟机器人移动直到遇到障碍物、到达终点或超过终点位置。

```java
class Solution {
    public boolean robot(String command, int[][] obstacles, int x, int y) {
        Map<Integer, Set<Integer>> map = new HashMap<>();
        for (int[] obstacle : obstacles) {
            if (map.get(obstacle[0]) == null) map.put(obstacle[0], new HashSet<>());
            map.get(obstacle[0]).add(obstacle[1]);
        }

        int curX = 0, curY = 0;
        // 无限模拟
        while (curX <= x && curY <= y) {
            for (char c : command.toCharArray()) {
                if (c == 'U') curY++;
                else curX++;

                // 到达终点
                if (curX == x && curY == y) return true;
                // 遇到障碍物
                if (map.get(curX) != null && map.get(curX).contains(curY)) return false;
            }
        }
        // 无法到达
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(x + y)$，空间复杂度为$O(1)$。

执行用时：1329ms，在所有java提交中击败了13.66%的用户。

内存消耗：40MB，在所有java提交中击败了6.66%的用户。

## 官方解题

&emsp;暂无，密切关注。由于指令周期内的增量距离是固定的，社区采用这种周期性将机器人移动压缩在一个指令周期内，大大减少复杂度。其次判断障碍物也不通过真实模拟，而是判断是否可达。

```java
class Solution {
    public boolean robot(String command, int[][] obstacles, int x, int y) {
        // 计算一个指令周期的运行距离
        int deltaX = 0, deltaY = 0;
        for (char c : command.toCharArray()) {
            if (c == 'U') deltaY++;
            else deltaX++;
       }
       // 不可达
       if (!isReach(deltaX, deltaY, x, y, command)) return false;

       // 可达则检测是否有障碍物
       for (int[] obstacle : obstacles) {
           if (obstacle[0] <= x && obstacle[1] <= y && isReach(deltaX, deltaY, obstacle[0], obstacle[1], command)) return false;
       }
       return true;
    }

    private boolean isReach(int deltaX, int deltaY, int targetX, int targetY, String command) {
        // 循环指令数目
        int minLoop = Math.min(targetX / deltaX, targetY / deltaY);
        deltaX *= minLoop;
        deltaY *= minLoop;
        if (deltaX == targetX && deltaY == targetY) return true;

        for (char c : command.toCharArray()) {
            if (c == 'U') deltaY++;
            else deltaX++;

            if (deltaX == targetX && deltaY == targetY) return true;
        }
        return false;
    }
}
```

&emsp;时间复杂度为$O(NM)$，其中$N$为指令长度，$M$为数组长度；空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.3MB，在所有java提交中击败了22.38%的用户。