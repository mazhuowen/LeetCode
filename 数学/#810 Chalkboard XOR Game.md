[toc]

We are given non-negative integers `nums[i]` which are written on a chalkboard.  Alice and Bob take turns erasing exactly one number from the chalkboard, with Alice starting first.  If erasing a number causes the bitwise XOR of all the elements of the chalkboard to become 0, then that player loses.  (Also, we'll say the bitwise XOR of one element is that element itself, and the bitwise XOR of no elements is 0.)

Also, if any player starts their turn with the bitwise XOR of all the elements of the chalkboard equal to 0, then that player wins.

Return True if and only if Alice wins the game, assuming both players play optimally.



**Notes:**

- $1 \le N \le 1000$. 
- $0 \le \text{nums[i]} \le 2^{16}$.



## 题目解读

&emsp;两个人轮流玩游戏，每次移除其中一个数，如果移除后数组亦或值为0，则当前玩家输掉比赛，或者轮到当前玩家时，亦或为0则当前玩家胜利。规定一个数字的亦或值为其本身。

```java
class Solution {
    public boolean xorGame(int[] nums) {

    }
}
```

## 程序设计

* 根据规则，当数组起始亦或值为0时，先手玩家胜利；
* 若数组起始亦或值不为0，数组长度为偶数个，则每次先手玩家面对的剩余数字长度为偶数个。先手玩家必须选择一个值使得剩余数值亦或不为0，假设当前偶数个数值亦或值为$S$，则显然$S \ne 0$；假设$\forall\ 0 \le i \le k$，$S \oplus num_i = 0$，则偶数个$(S \oplus num_0) \oplus (S \oplus num_1) \cdots (S \oplus num_k) = 0$，又$(S \oplus num_0) \oplus (S \oplus num_1) \cdots (S \oplus num_k) = (S \oplus S \cdots S) \oplus (num_0 \oplus num_2 \cdots num_k)$，即$0 \oplus S \ne 0$，这与上面的假设矛盾，故偶数个值必然存在一个数使得其他数亦或不为0，每一轮先手玩家选择最优解可获胜。
* 对于后手玩家如果数组长度为奇数，则后手玩家面临偶数个数值，后手玩家必胜。
* 综上，起始亦或值为0，或数组长度为偶数，先手玩家必胜。

```java
class Solution {
    public boolean xorGame(int[] nums) {
        if (nums == null || nums.length == 0) throw new IllegalArgumentException("invalid param");
        int val = 0;
        for (int num : nums) val ^= num;

        return val == 0 || nums.length % 2 == 0;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.4MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述思路参考官方解题。