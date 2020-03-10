[toc]

Given a list of scores of different students, return the average score of each student's **top five scores** in **the order of each student's id**.

Each entry `items[i]` has `items[i][0]` the student's id, and `items[i][1]` the student's score.  The average score is calculated using integer division.



Note:

* $1 \le \text{items.length} \le 1000$
* $\text{items[i].length} == 2$
* The IDs of the students is between `1` to `1000`
* The score of the students is between `1` to `100`
* For each student, there are at least 5 scores



## 题目解读

&emsp;统计每个学生的最高的五科成绩的平均值，并按照学号输出。

```java
class Solution {
    public int[][] highFive(int[][] items) {

    }
}
```

## 程序设计

* 最基本的想法是利用堆排序，然后计算。

```java
class Solution {
    public int[][] highFive(int[][] items) {
        if(items == null || items.length == 0) {
            return items;
        }
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        for(int[] score : items) {
            queue.add(score);
        }
        List<int[]> temp = new LinkedList<>();
        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            int count = 4;
            // 将相同学号的输出，同时计算最高的5个
            while (!queue.isEmpty() && queue.peek()[0] == cur[0]) {
                if(count > 0) {
                    cur[1] += queue.peek()[1];
                }
                queue.poll();
                count--;
            }
            cur[1] /= 5;
            temp.add(cur);
        }
        int[][] res = new int[temp.size()][2];
        for(int i = 0; i < temp.size(); i++){
            res[i] = temp.get(i);
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2N)$，空间复杂度为$O(N)$。

执行用时：6ms，在所有java提交中击败了74.85%的用户。

内存消耗：41.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区方法思路类似主要有两种，一种是排序计算：

```java
class Solution {
    public int[][] highFive(int[][] items) {
        // 根据学号排序，学号相同根据成绩排序
        Arrays.sort(items,((a,b) -> ((a[0] == b[0]) ? b[1] - a[1] : a[0] - b[0])));
        int[][] ans = new int[items[items.length-1][0]][2];
        // 遍历计算
        for (int i = 0; i < items.length; i++) {
            if (i == 0 || items[i][0] != items[i-1][0]) {
                ans[items[i][0]-1][0] = items[i][0];
                for (int j = i; j < i+5; j++) ans[items[j][0]-1][1] += items[j][1];
                ans[items[i][0]-1][1] /= 5;          
                i += 4;
            } 
        }
        return ans;
    }
}
```

一种用字典维护每个学生的成绩，键是学号，值是长度为5的数组，模拟堆维护递增的序列。