[toc]

You are given a 2D matrix of size m x n, consisting of non-negative integers. You are also given an integer k.

The value of coordinate (a, b) of the matrix is the XOR of all matrix[i][j] where 0 <= i <= a < m and 0 <= j <= b < n (0-indexed).

Find the kth largest value (1-indexed) of all the coordinates of matrix.

 

Example 1:

Input: matrix = [[5,2],[1,6]], k = 1
Output: 7
Explanation: The value of coordinate (0,1) is 5 XOR 2 = 7, which is the largest value.
Example 2:

Input: matrix = [[5,2],[1,6]], k = 2
Output: 5
Explanation: The value of coordinate (0,0) is 5 = 5, which is the 2nd largest value.
Example 3:

Input: matrix = [[5,2],[1,6]], k = 3
Output: 4
Explanation: The value of coordinate (1,0) is 5 XOR 1 = 4, which is the 3rd largest value.
Example 4:

Input: matrix = [[5,2],[1,6]], k = 4
Output: 0
Explanation: The value of coordinate (1,1) is 5 XOR 2 XOR 1 XOR 6 = 0, which is the 4th largest value.


Constraints:

m == matrix.length
n == matrix[i].length
1 <= m, n <= 1000
0 <= matrix[i][j] <= 106
1 <= k <= m * n



## 题目解读

&emsp;

```java
class Solution {
    public int kthLargestValue(int[][] matrix, int k) {

    }
}
```

## 程序设计

* 

```java
class Solution {
    public int kthLargestValue(int[][] matrix, int k) {
        int m = matrix.length, n = matrix[0].length;
        // 最小堆
        PriorityQueue<Integer> queue = new PriorityQueue<>(k);
        // 记录每行的亦或值
        int[] xor = new int[n];
        for (int i = 0; i < m; i++) {
            // 斜上方元素
            int pre = xor[0];
            // 每行第一个元素
            xor[0] ^= matrix[i][0];
            // 后续元素
            for (int j = 1; j < n; j++) {
                int tmp = xor[j];
                xor[j] ^= xor[j - 1] ^ pre ^ matrix[i][j];
                pre = tmp;
            }
            
            // 将当前亦或值放入堆中
            for (int j = 0; j < n; j++) {
                int num = xor[j];
                if (queue.size() == k && num > queue.peek()) {
                    queue.poll();
                }
                if (queue.size() < k) queue.add(num);
            }
        }
        return queue.poll();
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(MN\log_2K)$，空间复杂度为$O(K + N)$。



## 官方解题

&emsp;

```java

```

&emsp;时间复杂度为$O()$，空间复杂度为$O()$。
