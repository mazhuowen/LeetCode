[toc]

Given an array of integers `A`, find the sum of `min(B)`, where `B` ranges over every (contiguous) subarray of `A`.

Since the answer may be large, **return the answer modulo** $10^9 + 7$.



**Note:**

* $1 \le \text{A.length} \le 30000$
* $1 \le \text{A[i]} \le 30000$



## 题目解读

&emsp;求所有连续子区间最小值之和。

```java
class Solution {
    public int sumSubarrayMins(int[] A) {

    }
}
```

## 程序设计

* 如果数组中不存在重复数字，则可简单的遍历每个值，以当前值为区间最小值，向左右扩展，假设左右小于当前值的最长连续长度为`left`、`right`，则以当前值为最小值的区间组合数目为`(left + 1) * (right + 1)`；
* 考虑到重复数字，向左右扩展时，如果遇到相同值，若继续扩展，则计算的结果有重复，若不扩展，则会少计算结果。可遇到相同值向左继续扩展，而向右停止扩展，这样得到的结果不重复，时间复杂度为$O(N^2)$；
* 进一步简化模型，每次可以向左统计，而后继续遍历数组，当遇到比之前值小的数字时，表示之前值的右区间找到，此时可计算以之前数值为最小值的区间数目，可引入单调栈，保存之前的数字及其左区间计数。

```java
class Solution {
    static final int MOD = 1_000_000_007;

    public int sumSubarrayMins(int[] A) {
        if (A == null || A.length == 0) throw new IllegalArgumentException("invalid param");

        int res = 0;
        Stack<Pair> stack = new Stack<>();
        for (int i = 0; i < A.length; i++) {
            // 当前数字比栈顶小，则以栈顶为最小数字的区间结束，出栈计算
            while (!stack.isEmpty() && A[i] <= A[stack.peek().idx]) {
                Pair cur = stack.pop();
                res += A[cur.idx] * (cur.count + 1) * (i - cur.idx);
                res %= MOD;
            }

            // 统计当前数字左侧大于它的连续区间长度
            int count;
            if (stack.isEmpty()) count = i;
            else count = i - stack.peek().idx - 1;

            stack.push(new Pair(i, count));
        }

        // 栈中剩余的数字右区间为数组结尾
        while (!stack.isEmpty()) {
            Pair cur = stack.pop();
            res += A[cur.idx] * (cur.count + 1) * (A.length - cur.idx);
            res %= MOD;
        }
        return res;
    }
}

class Pair {
    int idx;
    int count;

    Pair(int idx, int count) {
        this.idx = idx;
        this.count = count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：29ms，在所有java提交中击败了60.73%的用户。

内存消耗：47.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方思路同上，实现细节不同。社区时间性能较好的方法不使用栈来确定左右边界，而是采用`KMP`中`next`数组动态规划的方式确定边界，并求值。

```java
class Solution {
    static final int MOD = 1_000_000_007;

    public int sumSubarrayMins(int[] A) {
        if (A == null || A.length == 0) throw new IllegalArgumentException("invalid param");

        // 左侧比当前值小的第一个索引
        int[] left = new int[A.length];
        for (int i = 0; i < A.length; i++) {
            int l = i - 1;
            // 当前左边界值较大，继续查找
            while (l >= 0 && A[l] >= A[i]) l = left[l];
            left[i] = l;
        }
        // 右侧比当前值小的第一个索引
        int[] right = new int[A.length];
        for (int i = A.length - 1; i >= 0; i--) {
            int r = i + 1;
            // 当前左边界值较大，继续查找
            while (r < A.length && A[r] > A[i]) r = right[r];
            right[i] = r;
        }

        int res = 0;
        for (int i = 0; i < A.length; i++) {
            res = (res + A[i] * (i - left[i]) * (right[i] - i)) % MOD;
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：9ms，在所有java提交中击败了96.10%的用户。

内存消耗：47.6MB，在所有java提交中击败了100.00%的用户。