[toc]

Given two numbers, `hour` and `minutes`. Return the smaller angle (in degrees) formed between the `hour` and the `minute` hand.



Constraints:

* $1 \le \text{hour} \le 12$
* $0 \le \text{minutes} \le 59$
* Answers within $10^{-5}$ of the actual value will be accepted as correct.



## 题目解读

&emsp;给定时钟上的时间，计算指针夹角。

```java
class Solution {
    public double angleClock(int hour, int minutes) {
        
    }
}
```

## 程序设计

* 需要注意时针在表盘上不会停留在整时，随着分钟进行而偏离。分针每分钟走6度，时针每小时走30度，每分钟走0.5度。

```java
class Solution {
    public double angleClock(int hour, int minutes) {
        if (hour < 1 || hour > 12 || minutes < 0 || minutes > 59) throw new IllegalArgumentException("invalid param");

        // 分针距离0的角度（每分钟走6度）
        double mAngle = minutes * 6;
        // 时针距离0的角度（整时的度数加分钟走过的度数）
        double hAngle = (hour % 12) * 30 + minutes * 0.5;

        // 计算差值
        double diff = Math.abs(hAngle - mAngle);
        return diff > 180 ? 360 - diff : diff;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.4MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。