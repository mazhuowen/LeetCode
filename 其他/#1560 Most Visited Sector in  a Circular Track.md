[toc]

Given an integer $n$ and an integer array `rounds`. We have a circular track which consists of $n$ sectors labeled from $1$ to $n$. A marathon will be held on this track, the marathon consists of $m$ rounds. The $i^{th}$ round starts at sector `rounds[i - 1]` and ends at sector `rounds[i]`. For example, round $1$ starts at sector `rounds[0]` and ends at sector `rounds[1]`

Return an array of the most visited sectors sorted in **ascending** order.

Notice that you circulate the track in ascending order of sector numbers in the counter-clockwise direction (See the first example).



**Constraints**:

* $2 \le n \le 100$
* $1 \le m \le 100$
* $\text{rounds.length} == m + 1$
* $1 \le \text{rounds[i]} \le n$
* $\text{rounds[i]} \ne \text{rounds[i + 1]}$ for $0 \le i < m$



## 题目解读

&emsp;给定起点和终点，求跑完全程后，经过最多的扇区，要求排序。

```java
class Solution {
    public List<Integer> mostVisited(int n, int[] rounds) {
        
    }
}
```

## 程序设计

* 仔细分析可知单周期起点至终点的路径经过次数比其他扇区多一。

```java
class Solution {
    public List<Integer> mostVisited(int n, int[] rounds) {
        List<Integer> res = new LinkedList<>();
        int start = rounds[0], end = rounds[rounds.length - 1];
        // 起点小于终点，起点到终点
        if (start <= end) {
            for (int i = start; i <= end; i++) res.add(i);
        } 
        // 起点大于终点，起点到最大扇区，最小扇区再到终点
        else {
            // 因为排序，颠倒顺序
            for (int i = 1; i <= end; i++) res.add(i); 
            for (int i = start; i <= n; i++) res.add(i);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：1ms，在所有java提交中击败了99.85%的用户。

内存消耗：40.2MB，在所有java提交中击败了13.34%的用户。

## 官方解题

&emsp;官方思路较为累赘，统计扇区经过次数，最后遍历得到最多次的扇区。