[toc]

Given an Android $3 \times 3$ key lock screen and two integers **m** and **n**, where $1 \le m \le n \le 9$, count the total number of unlock patterns of the Android lock screen, which consist of minimum of **m** keys and maximum **n** keys.

 

Rules for a valid pattern:

* Each pattern must connect at least **m** keys and at most **n** keys.
* All the keys must be distinct.
* If the line connecting two consecutive keys in the pattern passes through any other keys, the other keys must have previously selected in the pattern. No jumps through non selected key is allowed.
* The order of keys used matters.

<img src="../images/#351.png" style="zoom:120%;" />



## 题目解读

&emsp;给定上下限$m$和$n$，求解在这个区间的解锁手势数。

```java
class Solution {
    public int numberOfPatterns(int m, int n) {

    }
}
```

## 程序设计

* 首先需注意当前位置到下一个位置除了上下左右及对角线，还有相对距离为`(1,2)`的位置；其次需注意如果下一点已经被访问，则在当前点到下一点的直线上寻找第一个未访问的点。

```java
class Solution {
    int count;
    // 上下及对角线、斜线，总共16个组合
    int[] delta = new int[]{1, 0, -1, 0, 1, 1, -1, -1, 1, 2, 1, -2, -1, 2, -1, -2, 1};

    public int numberOfPatterns(int m, int n) {
        // 判断是否访问过
        boolean[] visited = new boolean[9];

        // 9个起点
        //  for (int i = 0; i < 9; i++) {
        //      numberOfPatterns(i, m, n, visited, 1);
        //  }
        // 事实上非中心点是类似的，不必每次回溯，乘４即可
        numberOfPatterns(0, m, n, visited, 1);
        numberOfPatterns(1, m, n, visited, 1);
        count *= 4;
        // 中心点
        numberOfPatterns(4, m, n, visited, 1);
        return count;
    }

    // num为目前路径的数目
    private void numberOfPatterns(int start, int m, int n, boolean[] visited, int num) {
        if (visited[start] || num > n) return;
        if (num >= m && num <= n) count++;

        // 试探
        visited[start] = true;
        int row = start / 3, col = start % 3;

        for (int i = 0; i < delta.length - 1; i++) {
            int x = row + delta[i], y = col + delta[i + 1];
            // 新坐标非法
            if (x < 0 || x > 2 || y < 0 || y > 2) continue;
            // 如果新坐标被访问过，根据题意，在该方向继续查找，直到找到一个未访问的有效点
            while (visited[x * 3 + y]) {
                x += delta[i];
                y += delta[i + 1];
                if (x < 0 || x > 2 || y < 0 || y > 2) break;
            }
            if (x < 0 || x > 2 || y < 0 || y > 2) continue;
            numberOfPatterns(3 * x + y, m, n, visited, num + 1);
        }
        // 回溯
        visited[start] = false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(Ｎ!)$，空间复杂度为$O(N)$。

执行用时：36ms，在所有java提交中击败了55.52%的用户。

内存消耗：36.2MB，在所有java提交中击败了50.00%的用户。

## 官方解题

&emsp;官方解题如下：

```java
public class Solution {

    private boolean used[] = new boolean[9];

    public int numberOfPatterns(int m, int n) {	        
        int res = 0;
        for (int len = m; len <= n; len++) {	            
            res += calcPatterns(-1, len);
            for (int i = 0; i < 9; i++) {	                
                used[i] = false;
            }            
        }
        return res;
    }

    private boolean isValid(int index, int last) {
        if (used[index])
            return false;
        // first digit of the pattern    
        if (last == -1)
            return true;
        // knight moves or adjacent cells (in a row or in a column)	       
        if ((index + last) % 2 == 1)
            return true;
        // indexes are at both end of the diagonals for example 0,0, and 8,8          
        int mid = (index + last)/2;
        if (mid == 4)
            return used[mid];
        // adjacent cells on diagonal  - for example 0,0 and 1,0 or 2,0 and //1,1
        if ((index%3 != last%3) && (index/3 != last/3)) {
            return true;
        }
        // all other cells which are not adjacent
        return used[mid];
    }

    private int calcPatterns(int last, int len) {
        if (len == 0)
            return 1;    
        int sum = 0;
        for (int i = 0; i < 9; i++) {
            if (isValid(i, last)) {
                used[i] = true;
                sum += calcPatterns(i, len - 1);
                used[i] = false;                    
            }
        }
        return sum;
    }
}
```