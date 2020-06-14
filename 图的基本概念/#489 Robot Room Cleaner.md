[toc]

Given a robot cleaner in a room modeled as a grid.

Each cell in the grid can be empty or blocked.

The robot cleaner with 4 given APIs can move forward, turn left or turn right. Each turn it made is 90 degrees.

When it tries to move into a blocked cell, its bumper sensor detects the obstacle and it stays on the current cell.

Design an algorithm to clean the entire room using only the 4 given APIs shown below.

```java
interface Robot {
  // returns true if next cell is open and robot moves into the cell.
  // returns false if next cell is obstacle and robot stays on the current cell.
  boolean move();

  // Robot will stay on the same cell after calling turnLeft/turnRight.
  // Each turn will be 90 degrees.
  void turnLeft();
  void turnRight();

  // Clean the current cell.
  void clean();
}
```



Notes:

* The input is only given to initialize the room and the robot's position internally. You must solve this problem "blindfolded". In other words, you must control the robot using only the mentioned 4 APIs, without knowing the room layout and the initial robot's position.
* The robot's initial position will always be in an accessible cell.
* The initial direction of the robot will be facing up.
* All accessible cells are connected, which means the all cells marked as 1 will be accessible by the robot.
* Assume all four edges of the grid are all surrounded by wall.



## 题目解读

&emsp;在不知道房间和机器人位置的情况下，利用`API`完成房间清理。

```java
/**
 * // This is the robot's control interface.
 * // You should not implement it, or speculate about its implementation
 * interface Robot {
 *     // Returns true if the cell in front is open and robot moves into the cell.
 *     // Returns false if the cell in front is blocked and robot stays in the current cell.
 *     public boolean move();
 *
 *     // Robot will stay in the same cell after calling turnLeft/turnRight.
 *     // Each turn will be 90 degrees.
 *     public void turnLeft();
 *     public void turnRight();
 *
 *     // Clean the current cell.
 *     public void clean();
 * }
 */

class Solution {
    public void cleanRoom(Robot robot) {
        
    }
}
```

## 程序设计

* 由于不知道房间分布和机器人位置，只能使用相对位置来表示遍历位置；其次不同于普通图的深度优先搜索，回溯时需要将机器人归位。

```java
class Solution {
    int[] delta = new int[]{0, 1, 0, -1, 0};
    Set<Pair> visited;

    public void cleanRoom(Robot robot) {
        this.visited = new HashSet<>();

        // 初始方向向上，及delta索引为3，相对坐标为(0,0)
        cleanRoom(robot, new Pair(0, 0), 3);
    }

    private void cleanRoom(Robot robot, Pair loc, int pre) {
        // 已遍历
        if (visited.contains(loc)) {
            back(robot);
            return;
        }

        robot.clean();
        visited.add(loc);

        // 从当前位置顺时针尝试四个方向
        for (int i = 0; i < 4; i++) {
            int direction = (pre + i) % 4;
            Pair newLoc = new Pair(loc.x + delta[direction], loc.y + delta[direction + 1]);
            if (robot.move()) cleanRoom(robot, newLoc, direction);

            // 顺时针旋转90度，为下次尝试作准备
            robot.turnRight();
        }

        // 回溯
        back(robot);
    }

    private void back(Robot robot) {
        robot.turnRight();
        robot.turnRight();
        robot.move();
        // 回到上一个位置还需要调转方向
        robot.turnRight();
        robot.turnRight();
    }
}

class Pair {
    int x;
    int y;

    Pair(int x, int y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public int hashCode() {
        return (x + "," + y).hashCode();
    }

    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof Pair)) return false;

        Pair other = (Pair)obj;
        if (other == this) return true;

        if (this.x == other.x && this.y == other.y) return true;
        else return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(NM)$。

执行用时：16ms，在所有java提交中击败了64.89%的用户。

内存消耗：41.3MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;同上。