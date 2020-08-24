[toc]

Given an array `arr` that represents a permutation of numbers from $1$ to $n$. You have a binary string of size $n$ that initially has all its bits set to zero.

At each step $i$ (assuming both the binary string and `arr` are 1-indexed) from $1$ to $n$, the bit at position `arr[i]` is set to $1$. You are given an integer $m$ and you need to find the latest step at which there exists a group of ones of length $m$. A group of ones is a contiguous substring of 1s such that it cannot be extended in either direction.

Return the latest step at which there exists a group of ones of length **exactly** $m$. If no such group exists, return $-1$.



## 题目解读

&emsp;给定数组`arr`，其中`arr[i]`表示将二进制数字第$i$位变为$1$的时间，求形成连续$m$个$1$的最后时间。

```java
class Solution {
    public int findLatestStep(int[] arr, int m) {
        
    }
}
```

## 程序设计

* 首先在得到这种类似“时间位置”数组时，可以转化为“位置时间”数组，以`[3,5,1,2,4]`为例，转化为“位置时间”数组为`[3,4,1,5,2]`，表示每个位置变为$1$的时间。
* 题目要求长度为$m$的连续$1$构成的序列实际就转变为窗口是$m$的序列中的最大值小于两侧邻接值，以$m=2$为例，遍历上述数组窗口，首先第一个窗口`3,4`最大值为$4$，只有右侧邻居为$1$，由于$4 \ge 1$，表示当前窗口构成的连续为$1$序列在时刻$4$时其右侧邻居已经是$1$了，从而长度大于$2$，不符合要求，继续遍历后继窗口；窗口`4,1`最大值是$4$，其左右邻居分别为$3$、$5$，依据上述分析，任然无法构成符合要求的长度，继续遍历；依次类推发现上述无符合要求的情况，最后结果为$-1$。
* 可见问题转化为：
  * 首先将数组转化为“位置时间”数组；
  * 求固定窗口为$m$的最大值，可参考[#239 Sliding Window Maximum](./队列/#239 Sliding Window Maximum.md)；
  * 遍历各个窗口，比较窗口左右邻居值与窗口最大值大小的关系，如果窗口最大值$max$小于左右邻居的最小值$min$，则说明可以形成要求的连续序列，最后时间就是$min - 1$；遍历记录所有答案中的最大值，得到结果。

```java
class Solution {
    public int findLatestStep(int[] arr, int m) {
        if (arr == null || arr.length == 0) throw new IllegalArgumentException("invalid param");

        int n = arr.length;
        if (m > n) return -1;
        if (m == n) return n;
        
        // 转化为时间数组
        int[] times = new int[n];
        int time = 1;
        for (int num : arr) times[num - 1] = time++;
        
        // 求m大小窗口的最大值
        int[] left = new int[n], right = new int[n];
        for (int i = 0; i < n; i++) {
            if (i % m == 0) left[i] = times[i];
            else left[i] = Math.max(left[i - 1], times[i]);

            int j = n - i - 1;
            if (j == n - 1 || (j + 1) % m == 0) right[j] = times[j];
            else right[j] = Math.max(right[j + 1], times[j]);
        }

        int res = 0;
        // 遍历窗口，比较窗口最大值与左右边界值的关系
        for (int i = 0; i <= n - m; i++) {
            int max = Math.max(left[i + m - 1], right[i]);
            // 最左边窗口，比较右侧边界
            if (i == 0 && times[m] > max) res = times[m];
            // 最右边窗口，比较左侧边界
            else if (i == n - m && times[n - m - 1] > max) res = Math.max(res, times[n - m - 1]);
            // 中间窗口，比较两侧边界
            else if (i > 0 && i < n - m && times[i - 1] > max && times[i + m] > max) res = Math.max(res, Math.min(times[i - 1], times[i + m])); 
        }

        return res - 1;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：12 ms

内存消耗：56.2 MB

## 官方解题

&emsp;暂无，密切关注。