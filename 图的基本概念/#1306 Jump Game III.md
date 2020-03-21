[toc]

Given an array of non-negative integers `arr`, you are initially positioned at `start` index of the array. When you are at index `i`, you can jump to `i + arr[i]` or `i - arr[i]`, check if you can reach to **any** index with value 0.

Notice that you can not jump outside of the array at any time.



**Constraints:**

- $1 \le \text{arr.length} \le 5 * 10^4$
- $0 \le \text{arr[i]} < \text{arr.length}$
- $0 \le start < \text{arr.length}$



## 题目解读

&emsp;给定非负数数组，根据一定规则跳到下一个索引，检查从指定位置出发是否可以到达任意一个值为0的索引。

```java
class Solution {
    public boolean canReach(int[] arr, int start) {

    }
}
```

## 程序设计

* 广度优先搜索的简单应用，记录访问过的索引，并根据规则生成下一个遍历的索引然后入队。

```java
class Solution {
    public boolean canReach(int[] arr, int start) {
        if (start < 0 || start >= arr.length || arr == null || arr.length == 0) throw new IllegalArgumentException("invalid param");

        boolean[] visited = new boolean[arr.length];
        Queue<Integer> queue = new LinkedList<>();
        queue.add(start);
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            if (arr[cur] == 0) return true;
            visited[cur] = true;
            // 根据规则生成
            int a = cur + arr[cur], b = cur - arr[cur];
            if (a < arr.length && !visited[a]) queue.add(a);
            if (b >= 0 && !visited[b]) queue.add(b);
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：42.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方采用栈实现了深度优先搜索。