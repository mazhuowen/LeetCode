[toc]

Given a matrix mat where every row is sorted in **strictly increasing** order, return the **smallest common element** in all rows.

If there is no common element, return `-1`.



**Constraints**:

* $1 \le \text{mat.length, mat[i].length} \le 500$
* $1 \le \text{mat[i][j]} \le 10^4$
* `mat[i]` is sorted in strictly increasing order.



## 题目解读

&emsp;给定矩阵，每行严格递增，求每行的最小公共数字。

```java
class Solution {
    public int smallestCommonElement(int[][] mat) {

    }
}
```

## 程序设计

* 类似归并排序，多路遍历。

```java
class Solution {
    public int smallestCommonElement(int[][] mat) {
        if (mat == null || mat.length == 0 || mat[0].length == 0) return -1;

        int m = mat.length, n = mat[0].length;

        // 当前遍历行
        int row = 0;
        // 当前遍历每行的列索引
        int[] cols = new int[m];
		// 与第一行值相等的数目，总共操作移动的数目
        int count = 0, move = 0;
        while (move < m * n) {
            if (row == 0) count = 0;
            int next = (row + 1) % m;
            
            // 当前行的值比下一行的值小，迭代遍历
            while (cols[row] < n && mat[row][cols[row]] < mat[next][cols[next]]) {
                cols[row]++;
                move++;
            }
            // 当前行无法匹配到与下一行相等的值
            if (cols[row] >= n) return -1;
            // 与第一行数值相等，计数
            if (mat[row][cols[row]] == mat[0][cols[0]]) count++;
            // 找到最小的公共数
            if (count == m) return mat[row][cols[row]];
            // 迭代
            row = next;
        }
        return -1;
    }
}
```

## 性能分析

&emsp;最坏情况时间复杂度为$O(M^2N)$，空间复杂度为$O(M)$。

执行用时：6ms，在所有java提交中击败了37.65%的用户。

内存消耗：61.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方还提供了其他三种思路。由于没有重复数字，最基本的是统计所有数值出现的频率，找到出现$M$此且数值最小的数就是答案。时间复杂度为$O(MN)$。

```java
class Solution {
    public int smallestCommonElement(int[][] mat) {
    	int count[] = new int[10001];
    	int n = mat.length, m = mat[0].length;
    	for (int i = 0; i < n; ++i) {
        	for (int j = 0; j < m; ++j) {
            	++count[mat[i][j]];
        	}
    	}
    	
        for (int k = 1; k <= 10000; ++k) {
        	if (count[k] == n) {
            	return k;
        	}
    	} 
    	return -1;
	}
}
```

&emsp;官方还提供了二分查找的思路，以第一行中的数字为目标，迭代查询后续行是否存在，如果都存在，说明是答案。

```java
class Solution {
    public int smallestCommonElement(int[][] mat) {
        int m = mat.length, n = mat[0].length;

        // 第一行的匹配数字
        int[] targets = mat[0];
        // 上次遍历到的位置，减少二分查找范围
        int[] limit = new int[m];

        for (int target : targets) {
            boolean flag = true;
            for (int i = 1; i < m; i++) {
                limit[i] = binarySearch(mat[i], target, limit[i], n - 1);
                // 当前列不存在target，表明target不是答案
                if (mat[i][limit[i]] != target) {
                    flag = false;
                    break;
                }
            }
            if (flag) return target;
        }
        return -1;
    }

    private int binarySearch(int[] array, int target, int left, int right) {
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (array[mid] >= target) right = mid;
            else left = mid + 1;
        }

        return left;
    }
}
```

&emsp;时间复杂度为$O(MN\log_2N)$，空间复杂度为$O(M)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：61.4MB，在所有java提交中击败了100.00%的用户。

&emsp;结合上述思路，改进最初的方法。首先最初的方法依靠上下行两两匹配，如果在迭代过程中存在多行一致，一行不一致的情况，需要再次遍历才会更新，浪费性能；可以记录本轮遍历的最大数字，然后其他行遍历查找，若未找到则更新最大数字继续遍历。其次采用二分查找来代替遍历。

```java
class Solution {
    public int smallestCommonElement(int[][] mat) {
        if (mat == null || mat.length == 0 || mat[0].length == 0) return -1;

        int m = mat.length, n = mat[0].length;

        // 当前遍历每行的列索引
        int[] cols = new int[m];
        // 当前遍历列最大值，及计数
        int curMax = mat[0][0], count = 0;
        while (true) {
            for (int i = 0; i < m; i++) {
                cols[i] = binarySearch(mat[i], curMax, cols[i], n);
                // 数组中不存在
                if (cols[i] == n) return -1;
                if (mat[i][cols[i]] == curMax) count++;
                else {
                    curMax = mat[i][cols[i]];
                    count = 1;
                }
                if (count == m) return curMax;
            }
        }
    }

    private int binarySearch(int[] array, int target, int left, int right) {
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (array[mid] >= target) right = mid;
            else left = mid + 1;
        }

        return left;
    }
}
```

&emsp;由于改进后都是有效移动，时间复杂度为$O(MN)$，空间复杂度为$O(M)$。

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户。

内存消耗：61.5 MB, 在所有 Java 提交中击败了100.00%的用户。