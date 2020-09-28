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

&emsp;



## 官方解题

&emsp;