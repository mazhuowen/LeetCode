[toc]

There are `N` network nodes, labelled `1` to `N`.

Given times, a list of travel times as **directed** edges `times[i] = (u, v, w)`, where `u` is the source node, `v` is the target node, and `w` is the time it takes for a signal to travel from source to target.

Now, we send a signal from a certain node `K`. How long will it take for all nodes to receive the signal? If it is impossible, return `-1`.



**Note**:

* `N` will be in the range `[1, 100]`.
* `K` will be in the range `[1, N]`.
* The length of `times` will be in the range `[1, 6000]`.
* All edges `times[i] = (u, v, w)` will have $1 \le u, v \le N$ and $0 \le w \le 100$.



## 题目解读

&emsp;计算信号从源点到每一个网络结点的最长传播时间。如果无法连通则返回`-1`。

```java
class Solution {
    public int networkDelayTime(int[][] times, int N, int K) {

    }
}
```

## 程序设计

* 典型的单源最短距离问题，计算源结点到每一个结点的距离，最后返回最大的即可。需要注意不连通的情况，此时遍历距离数组，如果还存在无限大的点，表示不连通。

```java
class Solution {
    public int networkDelayTime(int[][] times, int N, int K) {
        // 边少于连通的最低要求
        if (times == null || times.length < N - 1) return -1;

        // 构建图
        Node[] graph = new Node[N];
        for (int[] time : times) {
            graph[time[0] - 1] = new Node(time[1] - 1, time[2], graph[time[0] - 1]);
        }
        // 距离及前驱
        int[] distance = new int[N];
        int[] prev = new int[N];
        // 是否加入最短路径
        boolean[] flag = new boolean[N];
        // 初始化起始结点
        prev[K - 1] = K - 1;
        Arrays.fill(distance, Integer.MAX_VALUE);
        distance[K - 1] = 0;
        
        // 遍历N - 1次
        for (int i = 0; i < N - 1; i++) {
            // 查找当前未加入路径的最小距离
            int minIdx = -1;
            for (int j = 0; j < N; j++) {
                if (!flag[j] && (minIdx == -1 || distance[j] < distance[minIdx])) minIdx = j;
            }
            // 加入路径
            flag[minIdx] = true;
            // 更新后继结点距离
            Node temp = graph[minIdx];
            while (temp != null) {
                // 如果有更短的路径，更新距离及前驱
                if (!flag[temp.ver] && distance[minIdx] + temp.weight < distance[temp.ver]) {
                    distance[temp.ver] = distance[minIdx] + temp.weight;
                    prev[temp.ver] = minIdx;
                }
                temp = temp.next;
            }
        }
        // 最后遍历数组，检查是否存在不连通的点
        int maxDis = 0;
        for (int dis : distance) {
            maxDis = Math.max(maxDis, dis);
        }
        // 如果还有距离是不可达，则返回-1，否则返回最大值
        return maxDis == Integer.MAX_VALUE ? -1 : maxDis;
    }
}

class Node {
    int ver;
    int weight;
    Node next;

    Node(int ver, int weight, Node next) {
        this.ver = ver;
        this.weight = weight;
        this.next = next;
    }
}
```

* 优先级队列的实现方式如下：

```java
class Solution {
    public int networkDelayTime(int[][] times, int N, int K) {
        // 边少于连通的最低要求
        if (times == null || times.length < N - 1) return -1;

        // 构建图
        Node[] graph = new Node[N];
        for (int[] time : times) {
            graph[time[0] - 1] = new Node(time[1] - 1, time[2], graph[time[0] - 1]);
        }

        // 距离及前驱
        int[] prev = new int[N];
        int[] distance = new int[N];
        Arrays.fill(distance, Integer.MAX_VALUE);
        // 是否加入最短路径
        boolean[] flag = new boolean[N];

        // 保存数组。第一个位置为终点索引，第二个位置为前驱，第三个位置为到起点距离
        PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[2] - b[2]);
        queue.add(new int[]{K - 1, K - 1, 0});

        // 遍历N次
        for (int i = 0; i < N; i++) {
            // 出队已加入路径的点
            while (!queue.isEmpty() && flag[queue.peek()[0]]) {
                queue.poll();
            }
            if (queue.isEmpty()) return -1;
            // 查找当前未加入路径的最小距离
            int[] cur = queue.poll();
            int curIdx = cur[0], curPre = cur[1], curDis = cur[2];

            // 加入路径
            flag[curIdx] = true;
            distance[curIdx] = curDis;
            prev[curIdx] = curPre;

            // 更新后继结点距离
            Node temp = graph[curIdx];
            while (temp != null) {
                // 如果有更短的路径，更新距离及前驱
                if (!flag[temp.ver]) {
                   queue.add(new int[]{temp.ver, curIdx, curDis + temp.weight});
                }
                temp = temp.next;
            }
        }
        int maxDis = 0;
        for (int dis : distance) {
            maxDis = Math.max(maxDis, dis);
        }
        // 如果还有距离是不可达，则返回-1，否则返回最大值
        return maxDis == Integer.MAX_VALUE ? -1 : maxDis;
    }
}

class Node {
    int ver;
    int weight;
    Node next;

    Node(int ver, int weight, Node next) {
        this.ver = ver;
        this.weight = weight;
        this.next = next;
    }
}
```

## 性能分析

&emsp;基本形式时间复杂度为$O(V^2)$，空间复杂度为$O(V +Ｅ)$。

执行用时 :4 ms, 在所有 Java 提交中击败了91.59%的用户

内存消耗 :44.9 MB, 在所有 Java 提交中击败了96.00%的用户

&emsp;优先级队列形式时间复杂度为$O(VE)$，空间复杂度为$O(V + E)$。

执行用时：9ms，在所有java提交中击败了76.55%的用户。

内存消耗：44.2MB，在所有java提交中击败了98.00%的用户。

## 官方解题

&emsp;官方思路同上。