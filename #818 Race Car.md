[toc]

Your car starts at position 0 and speed +1 on an infinite number line.  (Your car can go into negative positions.)

Your car drives automatically according to a sequence of instructions A (accelerate) and R (reverse).

When you get an instruction "A", your car does the following: `position += speed, speed *= 2`.

When you get an instruction `"R"`, your car does the following: if your speed is positive then `speed = -1` , otherwise `speed = 1`.  (Your position stays the same.)

For example, after commands `"AAR"`, your car goes to positions `0->1->3->3`, and your speed goes to `1->2->4->-1`.

Now for some target position, say the **length** of the shortest sequence of instructions to get there.



**Note:**

- $1 \le target \le 10000$.



## 题目解读

&emsp;

```java
class Solution {
    public int racecar(int target) {

    }
}
```

## 程序设计

* 

```java

```

## 性能分析

&emsp;



## 官方解题

&emsp;