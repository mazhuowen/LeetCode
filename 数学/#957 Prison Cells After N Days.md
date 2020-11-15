[toc]

There are $8$ prison cells in a row, and each cell is either occupied or vacant.

Each day, whether the cell is occupied or vacant changes according to the following rules:

* If a cell has two adjacent neighbors that are both occupied or both vacant, then the cell becomes occupied.
* Otherwise, it becomes vacant.

(Note that because the prison is a row, the first and the last cells in the row can't have two adjacent neighbors.)

We describe the current state of the prison in the following way: `cells[i] == 1` if the i-th cell is occupied, else `cells[i] == 0`.

Given the initial state of the prison, return the state of the prison after $N$ days (and $N$ such changes described above.)



**Note:**

* $\text{cells.length} == 8$
* `cells[i]` is in `{0, 1}`
* $1 \le N \le 10^9$



## 题目解读

&emsp;给定起始状态，求$N$天后的状态。如果牢房相邻的房子被占用或空置，则当前牢房被占用；否则空置。

```java
class Solution {
    public int[] prisonAfterNDays(int[] cells, int N) {

    }
}
```

## 程序设计

* 第一个牢房和最后一个牢房由于不存在两个相连的房子，后续会变为$0$，其他房子可按照规则模拟。最基本的思路是模拟，但会超时。
* 由于只有$8$个格子，最多有$2^8$个状态，当$N$远远大于$256$时这个过程存在重复状态，而由于生成规则固定，如果存在重复状态，则必然存在循环周期，求出该周期简化运算。

```java
class Solution {
    public int[] prisonAfterNDays(int[] cells, int N) {
        if (N <= 0) throw new IllegalArgumentException("invalid param");
        // N从1开始，需减一从0开始
        N--;
        
        Map<Integer, int[]> record = new HashMap<>();
        // 计算第一个周期的值
        cells = next(cells);
        record.put(0, cells);
        // 记录原始状态
        int[] origin = cells;
        
        // 计算周期，如果N小于周期则提前跳出
        int epoch = 1;
        for (; epoch <= N; epoch++) {
            cells = next(cells);
            if (equals(origin, cells)) break;
            record.put(epoch, cells);
        }

        // N大于epoch，计算周期内的数值
        if (epoch <= N) N %= epoch;
        return record.get(N);
    }

    // 获取下一个状态
    private int[] next(int[] cells) {
        int[] tmp = new int[8];
        for (int i = 1; i < 7; i++) tmp[i] = cells[i - 1] == cells[i + 1] ? 1 : 0;
        return tmp;
    }

    // 判断是否相等
    private boolean equals(int[] arr1, int[] arr2) {
        for (int i = 0; i < 8; i++) if (arr1[i] != arr2[i]) return false;
        return true;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(1)$。

执行用时：2ms，在所有java提交中击败了70.69%的用户。

内存消耗：38.9MB，在所有java提交中击败了62.22%的用户。

## 官方解题

&emsp;官方思路类似，采用二进制表示压缩状态。