[toc]

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.



## 题目解读

&emsp;给定一个非负整数数组，最初位于数组的第一个位置。数组中的每个元素代表在该位置可以跳跃的最大长度。判断是否能够到达最后一个位置。

```java
class Solution {
    public boolean canJump(int[] nums) {

    }
}
```

## 程序设计

* 由于只是判断是否可到达终点，如果从头开始，则需要回溯，由于每个格子代表的是向后跳的最远距离，需要一个个尝试。换个思路，从表尾开始遍历判断元素，以`[2,3,1,1,4]`为例，1能到达4，则以1为目标点，向前遍历可以到达1的点，依次类推，最后2可到达3，起点可到达终点。
* 可见从后往前遍历，将可到达终点的点设为目标点，向前遍历判断可到达目标点的点，并更新目标点。这些目标点连成的路径不是最优路径，但本题只需判断是否可达。这样只需继续最近的目标点和遍历点，最后遍历到起点，只需判断起点是否可达最近的目标点。

```java
class Solution {
    public boolean canJump(int[] nums) {
        if (nums == null || nums.length <= 1) return true;

        // 记录目标点
        int target = nums.length - 1, start = target - 1;
        int offset = 1;
        while (start > 0) {
            // start可跳到target，设置target为start，继续向前遍历
            if (nums[start] >= offset) {
                target = start--;
                offset = 1;
            }
            // start跳不到target，向前检测
            else {
                start--;
                offset++;
            }
        }
        // 最后起点可达目标点，而目标点可达终点
        return nums[start] >= offset;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有java提交中击败了99.93%的用户。

内存消耗：41.7MB，在所有java提交中击败了8.84%的用户。

## 官方解题

&emsp;思路同上，官方解题更加简练。

```java
public class Solution {
    public boolean canJump(int[] nums) {
        int lastPos = nums.length - 1;
        for (int i = nums.length - 1; i >= 0; i--) {
            if (i + nums[i] >= lastPos) {
                lastPos = i;
            }
        }
        return lastPos == 0;
    }
}
```