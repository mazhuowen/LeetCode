[toc]

Given an array `A` of integers, a ramp is a tuple $(i, j)$ for which $i < j$ and $\text{A[i]} \le \text{A[j]}$.  The width of such a ramp is $j - i$.

Find the maximum width of a ramp in `A`.  If one doesn't exist, return $0$.



**Note:**

* $2 \le \text{A.length} \le 50000$
* $0 \le \text{A[i]} \le 50000$

 

## 题目解读

&emsp;求最大坡的宽度。

```java
class Solution {
    public int maxWidthRamp(int[] A) {

    }
}
```

## 程序设计

* 最基本的思路是暴力搜索，但是数据规模较大，会超时。可转变思路，使用索引数组根据对应值排序，这样问题就转变为索引的最大差值问题，只需遍历数组，记录之前出现的最小值即可。

```java
class Solution {
    public int maxWidthRamp(int[] A) {
        Integer[] index = new Integer[A.length];
        for (int i = 0; i < A.length; i++) index[i] = i;
        Arrays.sort(index, (a, b) -> A[a] - A[b]);

        int min = index[0], max = 0;
        for (int num : index) {
            max = Math.max(max, num - min);
            min = Math.min(min, num);
        }
        return max;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：43ms，在所有java提交中击败了49.08%的用户。

内存消耗：46.2MB，在所有java提交中击败了73.50%的用户。

## 官方解题

&emsp;官方还提供了二分查找的思路。官方巧妙从数组尾遍历，索引从高到低，而链表中只保存遍历过程中遇到的最大值，这样索引高的在链表起始，索引低的在链表尾部，而链表中的值却是递增的，可用二分查找查找大于等于遍历过程数值的第一个数，保证索引最大。

```java
import java.awt.Point;

class Solution {
    public int maxWidthRamp(int[] A) {
        int N = A.length;
        int ans = 0;
        List<Point> candidates = new ArrayList();
        candidates.add(new Point(A[N - 1], N - 1));
        
        for (int i = N - 2; i >= 0; i--) {
            // 二分查找大于等于第一个当前值的数值
            int lo = 0, hi = candidates.size();
            while (lo < hi) {
                int mi = lo + (hi - lo) / 2;
                if (candidates.get(mi).x < A[i]) lo = mi + 1;
                else hi = mi;
            }

            // 存在，计算坡度
            if (lo < candidates.size()) {
                int j = candidates.get(lo).y;
                ans = Math.max(ans, j - i);
            } 
            // 不存在，当前值最大，加入链表尾
            else {
                candidates.add(new Point(A[i], i));
            }
        }
        return ans;
    }
}
```

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：29ms，在所有java提交中击败了50.46%的用户。

内存消耗：45.5MB，在所有java提交中击败了95.58%的用户。

&emsp;社区采用单调栈的思路，使用栈维护严格递减序列，最大的坡起始位置必然在栈中，假设不在栈中，则此索引对应值必然大于栈中值，而栈中值索引在该值前面，故栈中索引是更好的起始点；在得到栈后，从数组尾开始遍历，出栈小于等于当前值的值（因为从尾部遍历，当前计算的长度必然是最长的，即使其前有符合条件的值，但是长度必然比当前值对应的坡短）。

```java
class Solution {
    public int maxWidthRamp(int[] A) {
        Stack<Integer> stack = new Stack<>();
        // 维护单调栈
        for (int i = 0; i < A.length; i++)  if (stack.isEmpty() || A[stack.peek()] > A[i]) stack.push(i);

        int res = 0;
        for (int i = A.length - 1; i >= 0; i--) {
            // 出栈计算
            while (!stack.isEmpty() && A[stack.peek()] <= A[i]) res = Math.max(res, i - stack.pop()); 
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：14ms，在所有java提交中击败了61.06%的用户。

内存消耗：46.2MB，在所有java提交中击败了78.55%的用户。