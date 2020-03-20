[toc]

There are `N` courses, labelled from `1` to `N`.

We are given `relations[i] = [X, Y]`, representing a prerequisite relationship between course `X` and course `Y`: course `X` has to be studied before course `Y`.

In one semester you can study any number of courses as long as you have studied all the prerequisites for the course you are studying.

Return the minimum number of semesters needed to study all courses. If there is no way to study all the courses, return `-1`.

**Note:**

* $1 \le N \le 5000$
* $1 \le \text{relations.length} \le 5000$
* $\text{relations[i][0]} != \text{relations[i][1]}$
* There are no repeated relations in the input.



## 题目解读

&emsp;课程的拓扑排序问题，题目限定没有自环，没有重复边。

```java
class Solution {
    public int minimumSemesters(int N, int[][] relations) {

    }
}
```

## 程序设计

* 图的拓扑排序，只是每次出队同层结点，而不是一个一个出队。记录出队层数，每一层就是一个学期要学的课程。

```java
class Solution {
    public int minimumSemesters(int N, int[][] relations) {
        if (N <= 0) return -1;
        // 无依赖关系，第一学期就可以学完
        if (relations == null || relations.length == 0) return 1;
        // 构建图
        Node[] graph = new Node[N];
        // 入度
        int[] degree = new int[N];
        for (int[] relation : relations) {
            // 检测参数
            graph[relation[1] - 1] = new Node(relation[0] - 1, graph[relation[1] - 1]);
            degree[relation[0] - 1]++;
        }
        // 初始化队列
        Queue<Integer> queue = new LinkedList<>();
        for (int i = 0; i < N; i++) {
            if (degree[i] == 0) queue.add(i);
        }
        // 存在环
        if (queue.isEmpty()) return -1;
        // 记录层数
        int count = 0;
        while (!queue.isEmpty()) {
            // 同层结点数
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int cur = queue.poll();
                Node temp = graph[cur];
                // 加入下一层无依赖结点
                while (temp != null) {
                    if (--degree[temp.ver] == 0) queue.add(temp.ver);
                    temp = temp.next;
                }
            }
            count++;
        }
        // 存在环
        for (int i = 0; i < N; i++) {
            if (degree[i] > 0) return -1;
        }
        return count;
    }
}

class Node {
    int ver;
    Node next;

    Node(int ver, Node next) {
        this.ver = ver;
        this.next = next;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(V + E)$，空间复杂度为$O(V + E)$。

执行用时：5ms，在所有java提交中击败了100.00%的用户。

内存消耗：41.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。