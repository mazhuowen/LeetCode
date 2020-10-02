[toc]

We have $n$ buildings numbered from $0$ to $n - 1$. Each building has a number of employees. It's transfer season, and some employees want to change the building they reside in.

You are given an array `requests` where `requests[i] = [fromi, toi]` represents an employee's request to transfer from building `fromi` to building `toi`.

**All buildings are full**, so a list of requests is achievable only if for each building, the **net change in employee transfers is zero**. This means the number of employees **leaving** is **equal** to the number of employees **moving in**. For example if $n = 3$ and two employees are leaving building $0$, one is leaving building $1$, and one is leaving building $2$, there should be two employees moving to building $0$, one employee moving to building $1$, and one employee moving to building $2$.

Return the maximum number of achievable requests.

 

**Constraints**:

* $1 \le n \le 20$
* $1 \le \text{requests.length} \le 16$
* $\text{requests[i].length} == 2$
* $0 \le \text{from}_i, \text{to}_i < n$



## 题目解读

&emsp;求使得入度、出度之差为$0$的可满足的最多请求数。

```java
class Solution {
    public int maximumRequests(int n, int[][] requests) {

    }
}
```

## 程序设计

* 粗略看似乎是构图然后删除环，最后统计不成环的边的数目。但该方法有一个难点就是必须删除最大的环，以图的邻接链表示$0 \to 1 \to 2$，$1 \to 0$，$2 \to 1$为例，有两个环，即$0 \to 1 \to 0$及$0 \to 2 \to 1 \to 0$，显然后一个环最大，删除最有利，故通过深度优先搜索删除图的思路不再适合。
* 参考社区思路，由于请求数目较少，可以暴力枚举所有请求，统计满足要求的最大请求数。

```java
class Solution {
    public int maximumRequests(int n, int[][] requests) {
        int stat = 1 << requests.length;
        int res = 0;
        // 枚举每个请求的选择状况
        for (int s = 1; s < stat; s++) {
            int[] degree = new int[n];
            // 对于选中的请求，统计度
            for (int j = 0; j < requests.length; j++) {
                if (((s >> j) & 1) == 0) continue;
                degree[requests[j][1]]++;
                degree[requests[j][0]]--;
            }

            boolean flag = true;
            // 统计度数是否为0
            for (int num : degree) {
                if (num != 0) {
                    flag = false;
                    break;
                }
            }
            if (flag) res = Math.max(res, Integer.bitCount(s));
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N2^N)$，空间复杂度为$O(N)$。

执行用时：133ms，在所有java提交中击败了39.05%的用户。

内存消耗：39.2MB，在所有java提交中击败了8.60%的用户。

## 官方解题

&emsp;暂无，密切关注。社区采用回溯尝试，其思路为尝试删除入度为负，即存在多余出边的点的某个边，优化策略主要有剪纸和尝试中的`return`，即遍历第一个入度为负数的点，然后尝试删除，本轮不再尝试其它入度为负数的点，因为入度为负数的点其后必然会尝试删除，从而避免大量的重复尝试，达到大大加速算法的功能。

```java
class Solution {
    int res = 0;
    public int maximumRequests(int n, int[][] requests) {
        // 构图并统计入度
        Node[] graph = new Node[n];
        int[] degree = new int[n];
        for (int[] request : requests) {
            if (request[0] == request[1]) continue;
            degree[request[0]]--;
            degree[request[1]]++;
            graph[request[0]] = new Node(request[1], graph[request[0]]);
        }
        backTracing(requests.length, graph, degree);
        return res;
    }
    
    private void backTracing(int cur, Node[] graph, int[] degree) {
        // 剪枝
        if (cur <= res) return;
        // 选择一个度非负的点，删除条件
        for (int i = 0; i < degree.length; i++) {
            if (degree[i] >= 0) continue;
            // 删除一条出边
            degree[i]++;
            for (Node tmp = graph[i], pre = null; tmp != null; pre = tmp, tmp = tmp.next) {
                if (tmp.ver == -1) continue;
                degree[tmp.ver]--;
                // 试探删除当前边
                int ver = tmp.ver;
                tmp.ver  = -1;
                backTracing(cur - 1, graph, degree);
                // 回溯
                tmp.ver = ver;
                degree[tmp.ver]++;
            }
            // 回溯
            degree[i]--;
            // 避免重复删除，加速回溯
            return;
        }
        // 所有点入度都如零，符合条件
        res = cur;
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

&emsp;时间复杂度为$O(NM)$，空间复杂度为$O(N + M)$。

执行用时：1ms，在所有java提交中击败了100.00%的用户。

内存消耗：36.8MB，在所有java提交中击败了68.47%的用户。