[toc]

A sorted list `A` contains 1, plus some number of primes.  Then, for every $p < q$ in the list, we consider the fraction $\frac{p}{q}$.

What is the `K`-th smallest fraction considered?  Return your answer as an array of ints, where `answer[0] = p` and `answer[1] = q`.

Note:

* A will have length between `2` and `2000`.
* Each `A[i]` will be between `1` and `30000`.
* `K` will be between `1` and $\text{A.length} * (\text{A.length} - 1) / 2$.



## 题目解读

&emsp;给定只包含素数的有序数组，得出第$k$小的分数组合。

```java
class Solution {
    public int[] kthSmallestPrimeFraction(int[] A, int K) {

    }
}
```

## 程序设计

* 由于数组是有序的，最小数和最大数组合的分数是最小的，次小值要么分母变小，要么分子变大，可以使用堆扩展的思路。

```java
class Solution {
    public int[] kthSmallestPrimeFraction(int[] A, int K) {
        if(A == null || A.length < 2 || K <= 0) {
            return new int[]{};
        }
        boolean[][] visit = new boolean[A.length][A.length];
        // 最小堆
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) ->
                Double.valueOf((double)A[a[0]] / A[a[1]]).compareTo((double) A[b[0]] / A[b[1]]));
        queue.add(new int[]{0, A.length - 1});
        // 加入堆标识，避免重复
        visit[0][A.length - 1] = true;
        int[] cur = queue.peek();
        while (K-- > 0) {
            cur = queue.poll();
            int i = cur[0];
            int j = cur[1];
            // 扩展
            if (j - 1 > i) {
                if(!visit[i][j - 1]) {
                    queue.add(new int[]{i, j - 1});
                    visit[i][j - 1] = true;
                }
                if(!visit[i + 1][j]) {
                    queue.add(new int[]{i + 1, j});
                    visit[i + 1][j] = true;
                }
            }
        }
        return new int[]{A[cur[0]], A[cur[1]]};
    }
}
```

* 官方对堆思路做了进一步优化，首先比较不用除法，采用分子、分母的积来比较；其次注意到与其一个坐标两两扩展，还不如加入所有分母和第一个素数的分数组合，然后只扩展分子索引，这样免去了多余的判断重复步骤。这样操作后堆中始终只有$N$个数据，大大简化流程。

```java
class Solution {
    public int[] kthSmallestPrimeFraction(int[] A, int K) {
        //采用积来比较
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a,b)->A[a[0]] * A[b[1]] - A[a[1]] * A[b[0]]);
        // 加入最小数和所有的分母组合
        for (int i = 1; i < A.length; ++i) pq.add(new int[]{0, i});
        while (--K > 0) {
            int[] frac = pq.poll();
            // 扩展分子索引
            if (frac[0]++ < frac[1]) pq.offer(frac);
        }
        int[] ans = pq.poll();
        return new int[]{A[ans[0]], A[ans[1]]};
    }
}
```

## 性能分析

&emsp;最小堆的方法时间复杂度为$O(K\log_2N)$，空间复杂度为$O(N)$。

执行用时：1437ms，在所有java提交中击败了10.81%的用户。

内存消耗：41.5MB，在所有java提交中击败了21.74%的用户。

## 官方解题

&emsp;除了堆思路，官方还提供了二分查找的思路。可使用二分法逼近第$k$小分数，并统计得到最接近这个值的组合，就是答案。注意采取如下策略，`high`保持大于等于$k$的区间，如果误差系数设置的太大，则`high`保持的大于等于$k$区间前得到的大于特定值的数超过$k$个，此时得到的组合偏大，需要精心设置此误差系数。

```java
class Solution {
    public int[] kthSmallestPrimeFraction(int[] A, int K) {
        if(A == null || A.length < 2 || K == 0) {
            return new int[]{};
        }
        int[] res = new int[2];
        // 分数界限
        double low = 0D, high = 1D;
        while(high - low > 1e-9) {
            double mid = low + (high - low) / 2;
            int[] count = counter(A, mid);
            // 当前分数mid大于超过K个数组中可组合的分数
            if(count[0] >= K) {
                high = mid;
                // 更新值
                res[0] = count[1];
                res[1] = count[2];
            } else {
                low = mid;
            }
        }
        return res;
    }

    private int[] counter(int[] A, double val) {
        int[] res = new int[]{0, 0, 1};
        // 分子索引（由于分母从小遍历，如果当前分子分母组合小于val，则当分母增大时组合仍然小于val，可以用一个值遍历，而不用每次遍历从头开始）
        int idx = 0;
        // 分母索引
        for(int i = 1; i < A.length; i++) {
            while(A[idx] < A[i] * val) idx++;

            // 当前分母i对应的小于val的分数组合（即最大的分子索引为idx-1，之前的分子都符合要求，共idx个）
            res[0] +=  idx;
            // 当前分母最后最接近val的组合如果比上一轮数值更大，更新
            if(idx >= 1 && res[1] * A[i] < A[idx - 1] * res[2]) {
                res[1] = A[idx - 1];
                res[2] = A[i];
            }
        }
        return res;
    }
}
```

&emsp;时间复杂度为$O(N\log_2W)$，空间复杂度为$O(1)$。其中$W$为二分区间，此处为$W = \frac{high - low}{error} = 10^9$，每次二分查找都要遍历数组，花费$N$。

执行用时：5ms，在所有java提交中击败了85.59%的用户。

内存消耗：41.3MB，在所有java提交中击败了21.74%的用户。