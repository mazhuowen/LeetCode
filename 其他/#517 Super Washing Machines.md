[toc]

You have $n$ super washing machines on a line. Initially, each washing machine has some dresses or is empty.

For each **move**, you could choose any $m$ ($1 \le m \le n$) washing machines, and pass **one** dress of each washing machine to one of its adjacent washing machines **at the same time** .

Given an integer array representing the number of dresses in each washing machine from left to right on the line, you should find the **minimum number of moves to** make all the washing machines have the same number of dresses. If it is not possible to do it, return -1.



**Note**:

* The range of $n$ is `[1, 10000]`.
* The range of dresses number in a super washing machine is `[0, 1e5]`.



## 题目解读

&emsp;初始时，每台机器都有一定量的衣服。在每一步操作中，可以选择任意位置的洗衣机，于此同时将每台洗衣机的一件衣服送到相邻的一台洗衣机。求使得洗衣机衣物相等的最小操作数。

```java
class Solution {
    public int findMinMoves(int[] machines) {

    }
}
```

## 程序设计

* 题目说明选中一台洗衣机，于此同时将每台洗衣机的一件衣服送到相邻的一台洗衣机，这个过程可以是单向，即向一个方向传播，也可以是双向，及两侧都想选中的洗衣机传播，不存在选中的洗衣机向两侧传播，因为每次只能移动一件衣物。
* 如果以每台洗衣机作为观察点，如果左侧缺少衣服且右侧缺少衣服，则说明当前洗衣机需要向两侧分两次传播；如果一侧缺少衣服，另一侧多出衣服，则单向传播，次数为两侧绝对值的较大值；如果两侧都多出衣服，则两侧向当前位置传播，由于可以同时进行，次数为两侧绝对值的较大值。

```java
class Solution {
    public int findMinMoves(int[] machines) {
        if (machines == null || machines.length == 0) return 0;

        int n = machines.length;
        // 统计和
        int sum = 0;
        for (int num : machines) sum += num;
        if (sum % n != 0) return -1;

        int target = sum / n;
        // 累积和
        int preSum = 0, res = 0;
        for (int i = 0; i < n; i++) {
            // i左侧需要衣服（正数表示多出衣服，负数表示少衣服）
            int left = preSum - i * target;
            // i右侧需要衣服
            int right = (sum - preSum - machines[i]) - (n - i - 1) * target;
            // 当前位置向两侧传播
            if (left < 0 && right < 0) res = Math.max(res, -left - right);
            // 其他情况
            else res = Math.max(res, Math.max(Math.abs(left), Math.abs(right)));

            preSum += machines[i];
        }

        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了77.27%的用户。

内存消耗：40.3MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;官方提供了另一种思路，即每次对比当前洗衣机需要移动的次数和之前所有洗衣机需要移动的次数。

```java
class Solution {
    public int findMinMoves(int[] machines) {
        if (machines == null || machines.length == 0) return 0;

        int n = machines.length;
        // 统计和
        int sum = 0;
        for (int num : machines) sum += num;
        if (sum % n != 0) return -1;

        int target = sum / n;
        for (int i = 0; i < n; i++) machines[i] -= target;
        // 累积和
        int preSum = 0, res = 0;
        for (int machine : machines) {
            preSum += machine;
            res = Math.max(res, Math.max(machine, Math.abs(preSum)));
        }

        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了77.27%的用户。

内存消耗：40.4MB，在所有java提交中击败了50.00%的用户。