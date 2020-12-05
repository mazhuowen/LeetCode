[toc]

Given an integer array `instructions`, you are asked to create a sorted array from the elements in `instructions`. You start with an empty container nums. For each element from **left to right** in `instructions`, insert it into `nums`. The **cost** of each insertion is the **minimum** of the following:

* The number of elements currently in `nums` that are **strictly less than** `instructions[i]`.
* The number of elements currently in `nums` that are **strictly greater than** `instructions[i]`.

For example, if inserting element $3$ into `nums = [1,2,3,5]`, the **cost** of insertion is $\min(2, 1)$ (elements $1$ and $2$ are less than $3$, element $5$ is greater than $3$) and nums will become $[1,2,3,3,5]$.

Return the **total cost** to insert all elements from `instructions` into `nums`. Since the answer may be large, return it **modulo** $10^9 + 7$

 

**Example 1**:

```
Input: instructions = [1,5,6,2]
Output: 1
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 5 with cost min(1, 0) = 0, now nums = [1,5].
Insert 6 with cost min(2, 0) = 0, now nums = [1,5,6].
Insert 2 with cost min(1, 2) = 1, now nums = [1,2,5,6].
The total cost is 0 + 0 + 0 + 1 = 1.
```

**Example 2**:

```
Input: instructions = [1,2,3,6,5,4]
Output: 3
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 2 with cost min(1, 0) = 0, now nums = [1,2].
Insert 3 with cost min(2, 0) = 0, now nums = [1,2,3].
Insert 6 with cost min(3, 0) = 0, now nums = [1,2,3,6].
Insert 5 with cost min(3, 1) = 1, now nums = [1,2,3,5,6].
Insert 4 with cost min(3, 2) = 2, now nums = [1,2,3,4,5,6].
The total cost is 0 + 0 + 0 + 0 + 1 + 2 = 3.
```

**Example 3**:

```
Input: instructions = [1,3,3,3,2,4,2,1,2]
Output: 4
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3,3].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3,3,3].
Insert 2 with cost min(1, 3) = 1, now nums = [1,2,3,3,3].
Insert 4 with cost min(5, 0) = 0, now nums = [1,2,3,3,3,4].
Insert 2 with cost min(1, 4) = 1, now nums = [1,2,2,3,3,3,4].
Insert 1 with cost min(0, 6) = 0, now nums = [1,1,2,2,3,3,3,4].
Insert 2 with cost min(2, 4) = 2, now nums = [1,1,2,2,2,3,3,3,4].
The total cost is 0 + 0 + 0 + 0 + 1 + 0 + 1 + 0 + 2 = 4.
```



**Constraints**:

* $1 \le \text{instructions.length} \le 10^5$
* $1 \le \text{instructions[i]} \le 10^5$



## 题目解读

&emsp;给定数组，从左往右插入数组数字使插入的新数组是有序数组，每次插入的代价为$\min(严格小于当前数的数字数目,严格大于当前数的数字数目)$，求最后的代价。

```java
class Solution {
    public int createSortedArray(int[] instructions) {

    }
}
```

## 程序设计

* 可使用树状数组。

```java
class Solution {
    private static final int MOD = 1_000_000_007;

    public int createSortedArray(int[] instructions) {
        int max = 0;
        for (int num : instructions) max = Math.max(max, num);

        int score = 0;
        BinaryIndexedTree tree = new BinaryIndexedTree(max + 1);
        for (int num : instructions) {
            score = (score + Math.min(tree.rangeSum(1, num - 1), tree.rangeSum(num + 1, max))) % MOD;
            tree.update(num, 1);
        }
        return score;
    }
}

class BinaryIndexedTree {
    int[] bitArr;

    BinaryIndexedTree(int n) {
        this.bitArr = new int[n + 1];
    }

    private static int lowBit(int x) {
        return x & -x;
    }

    public void update(int idx, int delta) {
        idx++;
        while (idx < bitArr.length) {
            bitArr[idx] += delta;
            idx += lowBit(idx);
        }
    }

    public int prefixSum(int idx) {
        idx++;
        int sum = 0;
        while (idx > 0) {
            sum += bitArr[idx];
            idx -= lowBit(idx);
        }
        return sum;
    }

    public int rangeSum(int fromIdx, int toIdx) {
        if (fromIdx > toIdx) return 0;
        return prefixSum(toIdx) - prefixSum(fromIdx - 1);
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2C)$，空间复杂度为$O(C)$。

执行用时：53 ms, 在所有 Java 提交中击败了81.57%的用户

内存消耗：50.7 MB, 在所有 Java 提交中击败了87.37%的用户。

## 官方解题

&emsp;暂无，密切关注。社区还采用线段树。

```java
class Solution {
    private static final int MOD = 1_000_000_007;

    public int createSortedArray(int[] instructions) {
        int min = Integer.MAX_VALUE, max = 0;
        for (int num : instructions) {
            min = Math.min(min, num);
            max = Math.max(max, num);
        }

        int score = 0;
        SegmentTree tree = new SegmentTree(min, max);
        for (int num : instructions) {
            score = (score + Math.min(tree.rangeSum(1, num - 1), tree.rangeSum(num + 1, max))) % MOD;
            tree.update(num, 1);
        }
        return score;
    }
}

class SegmentTree {
    int start, end;
    int sum;
    SegmentTree left, right;

    SegmentTree(int start, int end) {
        this.start = start;
        this.end = end;
    }

    private int getMid() {
        return start + (end - start) / 2;
    }

    private SegmentTree left() {
        if (this.left == null) this.left = new SegmentTree(start, getMid());
        return this.left;
    }

    private SegmentTree right() {
        if (this.right == null) this.right = new SegmentTree(getMid() + 1, end);
        return this.right;
    }

    public void update(int num, int delta) {
        if (end < num || start > num) return;
        if (start == end) {
            this.sum += delta;
            return;
        }
        this.left().update(num, delta);
        this.right().update(num, delta);
        this.sum = this.left().sum + this.right().sum;
    }

    public int rangeSum(int s, int e) {
        if (start > e || end < s) return 0;
        if (start >= s && end <= e) return sum;
        return this.left().rangeSum(s, e) + this.right().rangeSum(s, e);
    }
}
```

