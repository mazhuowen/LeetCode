[toc]

In a given integer array `A`, we must move every element of `A` to either list `B` or list `C`. (`B` and `C` initially start empty.)

Return true if and only if after such a move, it is possible that the average value of `B` is equal to the average value of `C`, and `B` and `C` are both non-empty.



Note:

* The length of `A` will be in the range `[1, 30]`.
* `A[i]` will be in the range of `[0, 10000]`.



## 题目解读

&emsp;尝试将数组分为均值相等的两部分。

```java
class Solution {
    public boolean splitArraySameAverage(int[] A) {

    }
}
```

## 程序设计

* 参考社区思路，根据子数组长度回溯尝试是否存在满足条件的组合。关键思路在于子数组长度所满足的子数组和必须是整数，从而剪枝加速遍历。
* 该思路在类似`[60,30,30,30,…]`的数组会超时，存在重复尝试。

```java
class Solution {
    public boolean splitArraySameAverage(int[] A) {
        if (A == null || A.length <= 1) return false;

        // 计算数组和
        int n = A.length, sum = 0;
        for (int num : A) sum += num;

        // 尝试子数组的长度
        for (int len = 1; len <= n / 2; len++) {
            // 子数组和必须为整数，剪枝
            if (sum * len % n != 0) continue;
            if (backTrace(A, 0, len, sum * len / n)) return true;
        }

        return false;
    }

    // 回溯尝试
    private boolean backTrace(int[] num, int idx, int targetLen, int targetSum) {
        if (targetLen == 0 && targetSum == 0) return true;
        if (targetLen == 0) return false;

        for (int i = idx; i <= num.length - targetLen; i++) {
            if (backTrace(num, i + 1, targetLen - 1, targetSum - num[i])) return true;
        }
        return false;
    }
}
```

* 对于连续相等的值，只需尝试第一个即可，避免重复尝试。

```java
class Solution {
    public boolean splitArraySameAverage(int[] A) {
        if (A == null || A.length <= 1) return false;

        // 计算数组和
        int n = A.length, sum = 0;
        for (int num : A) sum += num;

        // 排序使得相等的数在一起
        Arrays.sort(A);
        // 尝试子数组的长度
        for (int len = 1; len <= n / 2; len++) {
            // 优化1：子数组和必须为整数，剪枝
            if (sum * len % n != 0) continue;
            if (backTrace(A, 0, len, sum * len / n)) return true;
        }

        return false;
    }

    private boolean backTrace(int[] num, int idx, int targetLen, int targetSum) {
        
        if (targetLen == 0 && targetSum == 0) return true;
        if (targetLen == 0) return false;

        for (int i = idx; i <= num.length - targetLen; i++) {
            // 优化2：连续值不重复尝试
            if (i > idx && num[i] == num[i - 1]) continue;
            if (backTrace(num, i + 1, targetLen - 1, targetSum - num[i])) return true;
        }
        return false;
    }
}
```

## 性能分析

&emsp;回溯时间复杂度为$O(N2^N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：37.1MB，在所有java提交中击败了42.86%的用户。

## 官方解题

&emsp;官方思路采用数学的方式，时间复杂度为$O(2^N)$。

```java
import java.awt.Point;

class Solution {
    public boolean splitArraySameAverage(int[] A) {
        int N = A.length;
        int S = 0;
        for (int x: A) S += x;
        if (N == 1) return false;

        int g = gcd(S, N);
        Point mu = new Point(-(S/g), N/g);
        // A[i] -> fracAdd(A[i], mu)
        List<Point> A2 = new ArrayList();
        for (int x: A)
            A2.add(fracAdd(new Point(x, 1), mu));

        Set<Point> left = new HashSet();
        left.add(A2.get(0));
        for (int i = 1; i < N/2; ++i) {
            Set<Point> left2 = new HashSet();
            Point z = A2.get(i);
            left2.add(z);
            for (Point p: left) {
                left2.add(p);
                left2.add(fracAdd(p, z));
            }
            left = left2;
        }

        if (left.contains(new Point(0, 1))) return true;

        Set<Point> right = new HashSet();
        right.add(A2.get(N-1));
        for (int i = N/2; i < N-1; ++i) {
            Set<Point> right2 = new HashSet();
            Point z = A2.get(i);
            right2.add(z);
            for (Point p: right) {
                right2.add(p);
                right2.add(fracAdd(p, z));
            }
            right = right2;
        }

        if (right.contains(new Point(0, 1))) return true;

        Point sleft = new Point(0, 1);
        for (int i = 0; i < N/2; ++i)
            sleft = fracAdd(sleft, A2.get(i));

        Point sright = new Point(0, 1);
        for (int i = N/2; i < N; ++i)
            sright = fracAdd(sright, A2.get(i));

        for (Point ha: left) {
            Point ha2 = new Point(-ha.x, ha.y);
            if (right.contains(ha2) && (!ha.equals(sleft) || !ha2.equals(sright)))
                return true;
        }
        return false;
    }

    public Point fracAdd(Point A, Point B) {
        int numer = A.x * B.y + B.x * A.y;
        int denom = A.y * B.y;
        int g = gcd(numer, denom);
        numer /= g;
        denom /= g;

        if (denom < 0) {
            numer *= -1;
            denom *= -1;
        }

        return new Point(numer, denom);
    }

    public int gcd(int a, int b) {
       if (b==0) return a;
       return gcd(b, a%b);
    }
}
```

