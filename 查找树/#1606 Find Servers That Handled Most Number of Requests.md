[toc]

You have $k$ servers numbered from $0$ to $k-1$ that are being used to handle multiple requests simultaneously. Each server has infinite computational capacity but **cannot handle more than one request at a time**. The requests are assigned to servers according to a specific algorithm:

* The `i`th (0-indexed) request arrives.
* If all servers are busy, the request is dropped (not handled at all).
* If the `(i % k)`th server is available, assign the request to that server.
* Otherwise, assign the request to the next available server (wrapping around the list of servers and starting from 0 if necessary). For example, if the `i`th server is busy, try to assign the request to the `(i+1)`th server, then the `(i+2)`th server, and so on.

You are given a **strictly increasing** array `arrival` of positive integers, where `arrival[i]` represents the arrival time of the `i`th request, and another array `load`, where `load[i]` represents the load of the `i`th request (the time it takes to complete). Your goal is to find the **busiest server(s)**. A server is considered **busiest** if it handled the most number of requests successfully among all the servers.

Return a list containing the IDs (0-indexed) of the **busiest server(s)**. You may return the IDs in any order.



**Constraints**:

* $1 \le k \le 10^5$
* $1 \le \text{arrival.length, load.length} \le 10^5$
* $\text{arrival.length} == \text{load.length}$
* $1 \le \text{arrival[i], load[i]} \le 10^9$
* `arrival` is **strictly increasing**.



## 题目解读

&emsp;根据服务倒流规则找到最繁忙的服务。

```java
class Solution {
    public List<Integer> busiestServers(int k, int[] arrival, int[] load) {

    }
}
```

## 程序设计

* 最基本的思路是直接模拟，会超时。

```java
class Solution {
    public List<Integer> busiestServers(int k, int[] arrival, int[] load) {
        int[] times = new int[k], counter = new int[k];
        int maxCount = 0;
        for (int i = 0; i < arrival.length; i++) {
            int idx = i % k;
            // 查找空闲服务器
            if (times[idx] > arrival[i]) {
                do {
                    idx = (idx + 1) % k;
                } while (idx != i % k && times[idx] > arrival[i]);
                // 无空闲服务器，丢弃本次请求
                if (idx == i % k) continue; 
            }

            // 记录处理结束时间
            times[idx] = arrival[i] + load[i];
            maxCount = Math.max(maxCount, ++counter[idx]);
        }

        List<Integer> res = new LinkedList<>();
        for (int i = 0; i < k; i++) if (counter[i] == maxCount) res.add(i);
        return res;
    }
}
```

* 问题的优化点在于怎样快速查找符合条件的空闲服务器，一个思路是使用线段树保存服务器区间`(i,j)`内的最早结束时间，若当前索引为`idx`，则首先查询`idx`左侧满足条件的编号最小的服务器，如果不存在则循环查找其前，即`0~idx`满足条件的服务器，时间复杂度是$O(\log_2K)$；
* 设置服务器后只需将服务器值设置为当前处理结束值，并沿路更新祖先节点的最早结束时间，复杂度为$O(\log_2N)$。

```java

class Solution {
    public List<Integer> busiestServers(int k, int[] arrival, int[] load) {
        // 区间树
        SegmentTree root = new SegmentTree(0, k - 1);
        // 记录每个服务器调用次数
        int[] counter = new int[k];
        // 记录最大次数
        int maxCount = 0;

        for (int i = 0; i < arrival.length; i++) {
            // 查找右侧可用服务器
            int idx = root.getServer(i % k, k - 1, arrival[i]);
            // 右侧没有，则查找左侧
            if (idx == -1 && i % k > 0) idx = root.getServer(0, i % k - 1, arrival[i]);
            // 无可用服务器
            if (idx == -1) continue;
            // 更新时间及技术
            maxCount = Math.max(maxCount, ++counter[idx]);
            root.updateServer(idx, arrival[i] + load[i]);
        }

        List<Integer> res = new LinkedList<>();
        for (int i = 0; i < k; i++) if (counter[i] == maxCount) res.add(i);
        return res;
    }
}

class SegmentTree {
    // 服务器区间
    int start, end;
    // 最早结束时间
    int minTime;

    SegmentTree left;
    SegmentTree right;

    SegmentTree(int start, int end) {
        this.start = start;
        this.end = end;
        this.minTime = 0;
    }

    private int mid() {
        return (this.start + this.end) / 2;
    }

    private SegmentTree getLeft() {
        if (this.left == null) this.left = new SegmentTree(start, mid());
        return this.left;
    }

    private SegmentTree getRight() {
        if (this.right == null) this.right = new SegmentTree(mid() + 1, end);
        return this.right;
    }

    // 在指定区间获取结束时间比value小的服务器
    public int getServer(int s, int e, int value) {
        // 不存在或不在区间内
        if (this.minTime > value || this.end < this.start ||  s > this.end || e < this.start) return -1;
        // 单节点服务器
        if (this.start == this.end) return s;

        // 查询子节点
        int mid = mid();
        int leftServer = this.getLeft().getServer(s, Math.min(mid, e), value);
        // 找到满足条件的第一个节点，返回
        if (leftServer != -1) return leftServer;
        return this.getRight().getServer(Math.max(mid + 1, s), e, value);
    }

    // 更新服务器时间
    public int updateServer(int no, int time) {
        if (no > this.end || no < this.start) return this.minTime;
        if (this.end == this.start) {
            this.minTime = time;
            return time;
        }
        // 更新区间最早结束时间
        int l = this.getLeft().updateServer(no, time);
        int r = this.getRight().updateServer(no, time);
        this.minTime = Math.min(l, r);
        return this.minTime;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N\log_2K)$，空间复杂度为$O(K)$。

执行用时：94ms，在所有java提交中击败了100.00%的用户。

内存消耗：55MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，密切关注。社区较多采用的是红黑树或优先级队列动态操作，时间复杂度较高。