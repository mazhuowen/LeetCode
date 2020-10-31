[toc]

You have a number of envelopes with widths and heights given as a pair of integers `(w, h)`. One envelope can fit into another if and only if both the width and height of one envelope is greater than the width and height of the other envelope.

What is the maximum number of envelopes can you Russian doll? (put one inside other)



**Note**:
Rotation is not allowed.



## 题目解读

&emsp;俄罗斯套娃最多层数。


```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        
    }
}
```

## 程序设计

* 最基本的思路是按照宽度排序，宽度相同则按照高度排序，然后用数组记录当前位置最多嵌套层数。遍历位置，并更新后续可以装下当前位置的盒子的层数。

```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        if (envelopes == null || envelopes.length == 0) return 0;

        int n = envelopes.length;
        Arrays.sort(envelopes, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        int[] counter = new int[n];
        Arrays.fill(counter, 1);
        int max = 1;
        for (int i = 0; i < envelopes.length - 1; i++) {
            int num = counter[i];
            for (int j = i + 1; j < envelopes.length; j++) {
                // 后续盒子j可以装下当前盒子i，更新
                if (envelopes[j][0] > envelopes[i][0] && envelopes[j][1] > envelopes[i][1]) {
                    counter[j] = Math.max(counter[j], counter[i] + 1);
                    max = Math.max(max, counter[j]);
                }
            }
        }
        return max;
    }
}
```

* 参考官方思路，题目是[#300 Longest Increasing Subsequence](./#300 Longest Increasing Subsequence.md)的进阶；首先根据宽度排序，宽度相同则根据高度逆序排序；这样问题转化为最长递增子序列的问题，查找最长的高度递增子序列长度。

```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        if (envelopes == null || envelopes.length == 0) return 0;

        // 排序
        Arrays.sort(envelopes, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        return maxSubSeq(envelopes);
    }

    // 返回最大递增子序列
    private int maxSubSeq(int[][] envelopes) {
        int len = 0;
        int[] k = new int[envelopes.length];
        for (int i = 0; i < envelopes.length; i++) {
            int target = envelopes[i][1];
            if (len == 0 || target > k[len - 1]) k[len++] = envelopes[i][1] ;
            else {
                int left  = 0, right = len;
                while (left < right) {
                    int mid = left + (right - left) / 2;
                    if (k[mid] >= target) right = mid;
                    else left = mid + 1;
                }
                k[left] = target;
            }
        }
        return len;
    } 
}
```

## 性能分析

&emsp;暴力法时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：297ms，在所有java提交中击败了33.54%的用户。

内存消耗：40.7MB，在所有java提交中击败了100.00%的用。

&emsp;二分法时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：13ms，在所有java提交中击败了86.12%的用户。

内存消耗：41MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述参考官方思路。