[toc]

You are given an array `points`, an integer `angle`, and your `location`, where `location = [posx, posy]` and `points[i] = [xi, yi]` both denote **integral coordinates** on the `X-Y` plane.

Initially, you are facing directly east from your position. You **cannot move** from your position, but you can **rotate**. In other words, `posx` and `posy` cannot be changed. Your field of view in **degrees** is represented by `angle`, determining how wide you can see from any given view direction. Let `d` be the amount in degrees that you rotate counterclockwise. Then, your field of view is the **inclusive** range of angles `[d - angle/2, d + angle/2]`.



**Constraints**:

* $1 \le \text{points.length} \le 10^5$
* $\text{points[i].length} == 2$
* $\text{location.length} == 2$
* $0 \le \text{angle} < 360$
* $0 \le \text{posx, posy, xi, yi} \le 10^9$



## 题目解读

&emsp;判断视野内最多的点数。

```java
class Solution {
    public int visiblePoints(List<List<Integer>> points, int angle, List<Integer> location) {

    }
}
```

## 程序设计

* 首先计算所有点相对于观察点的角度，然后排序并排列滑动，保持队列中的角度在范围内；
* 与常见双指针不同，由于角度的循环性，右指针需要遍历结束后继续从头开始，这就需要判断防止重复循环；其次根据`arctan`计算时需要根据象限的不同处理，然后转为角度值。

```java
class Solution {
    public int visiblePoints(List<List<Integer>> points, int angle, List<Integer> location) {
        // 与位置一致的点
        int origin = 0, posX = location.get(0), posY = location.get(1);
        // 保存每个点到观察点的角度
        List<Double> angles = new ArrayList<>();
        // 计算角度
        for (List<Integer> point : points) {
            int x = point.get(0) - posX, y = point.get(1) - posY;
            if (x == 0 && y == 0) origin++;
            else if (x == 0) angles.add(y > 0 ? 90.0D : 270.0D);
            // 将弧度转换为角度
            else {
                // 根据坐标系还原角度值
                double arctan = Math.atan((double)y / x);
                // 二三象限
                if (x < 0) arctan += Math.PI;
                // 四象限
                else if (y <= 0) arctan += 2 * Math.PI;
                angles.add(Math.toDegrees(artan));
            }
        }

        Collections.sort(angles);
        // 双指针滑动判断
        int left = 0, right = 0;
        int max = 0;
        while (left < angles.size()) {
            if (right >= left) {
                if (angles.get(right) - angles.get(left) <= angle) {
                    right = (right + 1) % angles.size();
                    // 对于特殊情况即起始为0，遍历结束回到0的情况的处理
                    if (left == right) return angles.size() + origin;
                } else {
                    max = Math.max(max, right - left);
                    left++;
                }
            }
            else {
                double diff = 360 - angles.get(left) + angles.get(right);
                if (diff <= angle) {
                    right = (right + 1) % angles.size();
                    // 循环头尾相遇处理
                    if (left == right) return angles.size() + origin;
                } else {
                    max = Math.max(max, right + angles.size() - left);
                    left++;
                }
            }
        }
        return max + origin;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。



## 官方解题

&emsp;