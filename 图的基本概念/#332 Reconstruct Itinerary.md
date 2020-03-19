[toc]

Given a list of airline tickets represented by pairs of departure and arrival airports `[from, to]`, reconstruct the itinerary in order. All of the tickets belong to a man who departs from `JFK`. Thus, the itinerary must begin with `JFK`.

**Note:**

* If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string. For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.
* All airports are represented by three capital letters (IATA code).
* You may assume all tickets form at least one valid itinerary.



## 题目解读

&emsp;给定飞机出发和降落的机场地点，对一些列行程进行重新规划排序。题目限定从`JFK`出发。

> 需要向面试官咨询有向边是否存在多条，这是问题的关键。

```java
class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {

    }
}
```

## 程序设计

* 首先想到的是机场就是顶点，行程是有向边，由于乘客不能瞬移，必须由一个地方到另一个地方，如果这些行程属于同一个人，意味着行程组成的图可以从`JFK`开始遍历，遍历完所有的边。
* 根据这个思路，类似于顶点的深度优先搜索，通过将边标记为0实现边的深度优先搜索，代码如下：

```java
class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        List<String> res = new LinkedList<>();
        if (tickets == null || tickets.size() == 0) return res;

        // 结点集
        Set<String> verName = new HashSet<>();
        for (List<String> ticket : tickets) {
            verName.add(ticket.get(0));
            verName.add(ticket.get(1));
        }
        int verNum = verName.size();
        // 根据结点字母序排序，方便后面搜索先搜索排序靠前的结点
        String[] verIdx = verName.toArray(new String[verNum]);
        Arrays.sort(verIdx);
        // 结点名称索引映射
        Map<String, Integer> idxMap = new HashMap<>();
        for (int i = 0; i < verNum; i++) {
            idxMap.put(verIdx[i], i);
        }

        // 边数
        int edgeNum = 0;
        // 边集
        int[][] graph = new int[verNum][verNum];
        for (List<String> ticket : tickets) {
            int start = idxMap.get(ticket.get(0));
            int end = idxMap.get(ticket.get(1));
            if (graph[start][end] == 0) {
                graph[start][end] = 1;
                edgeNum++;
            }
        }
        // 从JFK出发深度优先搜索
        boolean flag = findPath(idxMap.get("JFK"), graph, edgeNum, verIdx, res);
        if (!flag) throw new IllegalArgumentException("error graph");
        Collections.reverse(res);
        return res;
    }

    private boolean findPath(int start, int[][] graph, int edgeNum, String[] verIdx, List<String> res) {
        // 当前结点start是否不存在出边
        boolean flag = true;
        for (int i = 0; i < graph.length; i++) {
            // 存在边则遍历
            if (graph[start][i] == 1) {
                flag = false;
                // 已遍历该边，标记为0断开
                graph[start][i] = 0;
                // 后继结点遍历完所有的边，路径存在，添加当前结点，返回
                if (findPath(i, graph, edgeNum - 1, verIdx, res)) {
                    res.add(verIdx[start]);
                    return true;
                }
                // 后继结点无法遍历完所有的边，重新连接边
                graph[start][i] = 1;
            }
        }
        // 当前结点不存在出边且总的可遍历边数为0，表示找到路径，且当前结点是最终结点
        if (flag && edgeNum == 0) {
            res.add(verIdx[start]);
            return true;
        }
        return false;
    }
}
```

* 上述代码测试用例不通过，因为上述思路默认边是不重复的，测试用例`[["EZE","AXA"],["TIA","ANU"],["ANU","JFK"],["JFK","ANU"],["ANU","EZE"],["TIA","ANU"],["AXA","TIA"],["TIA","JFK"],["ANU","TIA"],["JFK","TIA"]]`不通过，思路不变，可以通过在邻接矩阵中的计数表示边的条数来实现，改动如下：

```java
class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        List<String> res = new LinkedList<>();
        if (tickets == null || tickets.size() == 0) return res;

        // 结点集
        Set<String> verName = new HashSet<>();
        for (List<String> ticket : tickets) {
            verName.add(ticket.get(0));
            verName.add(ticket.get(1));
        }
        int verNum = verName.size();
        // 根据结点字母序排序，方便后面搜索先搜索排序靠前的结点
        String[] verIdx = verName.toArray(new String[verNum]);
        Arrays.sort(verIdx);
        // 结点名称索引映射
        Map<String, Integer> idxMap = new HashMap<>();
        for (int i = 0; i < verNum; i++) {
            idxMap.put(verIdx[i], i);
        }

        // 边数
        int edgeNum = 0;
        // 边集
        int[][] graph = new int[verNum][verNum];
        for (List<String> ticket : tickets) {
            int start = idxMap.get(ticket.get(0));
            int end = idxMap.get(ticket.get(1));
            graph[start][end]++;
            edgeNum++;
        }
        // 从JFK出发深度优先搜索
        boolean flag = findPath(idxMap.get("JFK"), graph, edgeNum, verIdx, res);
        if (!flag) throw new IllegalArgumentException("error graph");
        Collections.reverse(res);
        return res;
    }

    private boolean findPath(int start, int[][] graph, int edgeNum, String[] verIdx, List<String> res) {
        // 当前结点start是否不存在出边
        boolean flag = true;
        for (int i = 0; i < graph.length; i++) {
            // 存在边则遍历
            if (graph[start][i] > 0) {
                flag = false;
                // 已遍历该边，标记为0断开
                graph[start][i]--;
                // 后继结点遍历完所有的边，路径存在，添加当前结点，返回
                if (findPath(i, graph, edgeNum - 1, verIdx, res)) {
                    res.add(verIdx[start]);
                    return true;
                }
                // 后继结点无法遍历完所有的边，重新连接边
                graph[start][i]++;
            }
        }
        // 当前结点不存在出边且总的可遍历边数为0，表示找到路径，且当前结点是最终结点
        if (flag && edgeNum == 0) {
            res.add(verIdx[start]);
            return true;
        }
        return false;
    }
}
```

* 邻接表实现如下：

```java
class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        List<String> res = new LinkedList<>();
        if (tickets == null || tickets.size() == 0) return res;

        // 结点集
        Set<String> verName = new HashSet<>();
        for (List<String> ticket : tickets) {
            verName.add(ticket.get(0));
            verName.add(ticket.get(1));
        }
        int verNum = verName.size();
        // 根据结点字母序排序，方便后面搜索先搜索排序靠前的结点
        String[] verIdx = verName.toArray(new String[verNum]);
        Arrays.sort(verIdx);
        // 结点名称索引映射
        Map<String, Integer> idxMap = new HashMap<>();
        for (int i = 0; i < verNum; i++) {
            idxMap.put(verIdx[i], i);
        }

        // 边数
        int edgeNum = tickets.size();
        // 边集
        Node[] graph = new Node[verNum];
        for (List<String> ticket : tickets) {
            int start = idxMap.get(ticket.get(0));
            int end = idxMap.get(ticket.get(1));
            // 插入保持索引的有序性
            if (graph[start] == null || graph[start].val >= end) {
                graph[start] = new Node(end, graph[start]);
            } else {
                Node pre = null;
                Node temp = graph[start];
                while (temp != null && temp.val < end) {
                    pre = temp;
                    temp = temp.next;
                }
                pre.next = new Node(end, temp);
            }
        }
        // 从JFK出发深度优先搜索
        boolean flag = findPath(idxMap.get("JFK"), graph, edgeNum, verIdx, res);
        // 不存在可行路径
        if (!flag) throw new IllegalArgumentException("error graph");
        Collections.reverse(res);
        return res;
    }

    private boolean findPath(int start, Node[] graph, int edgeNum, String[] verIdx, List<String> res) {
        // 当前结点start是否不存在出边
        boolean flag = true;
        Node temp = graph[start];
        Node pre = null;
        while (temp != null) {
            flag = false;
            // 暂时删除
            if (pre != null) {
                pre.next = temp.next;
            } else {
                graph[start] = temp.next;
            }
            // 后继结点遍历完所有的边，路径存在，添加当前结点，返回
            if (findPath(temp.val, graph, edgeNum - 1, verIdx, res)) {
                res.add(verIdx[start]);
                return true;
            }
            // 插入保持有序性，插入到原来位置
            if (pre == null) {
                temp.next = graph[start];
                graph[start] = temp;
            } else {
                temp.next = pre.next;
                pre.next = temp;
            }
            pre = temp;
            temp = temp.next;
        }

        // 当前结点不存在出边且总的可遍历边数为0，表示找到路径，且当前结点是最终结点
        if (flag && edgeNum == 0) {
            res.add(verIdx[start]);
            return true;
        }
        return false;
    }
}

class Node {
    int val;
    Node next;

    Node(int val) {
        this.val = val;
    }

    Node(int val, Node next) {
        this.val = val;
        this.next = next;
    }
}
```

* 事实上每条边走一遍，且走完全部的边，这是一道有向边的欧拉路径问题。无向图中欧拉回路满足条件为任意一个结点度为偶数，欧拉路径满足条件为有且只有两个结点度为奇数，分别为起始点和结束点，其它点都是偶数。
* 应用到有向图中，可知如果存在欧拉回路，则必然每个结点的入度和出度一样，存在欧拉路径则起始点的出度比入度多一，结束点出度比入度少一，其它点出度、入度相同。自然而然想到使用求解欧拉回路的方法，即深度遍历并拼接环的方法，这是可行的，由于本题规定了一定存在路径，事实上要么有向图是欧拉回路，从起始点出发是由多个环构成；要么是欧拉路径，除了多个环，还有一个链通向结束结点。
* 这样思路就是从起始点出发，每次选取最优路径，遍历完边后删除即可；这样会得到一条链路，如果是欧拉回路，则会遍历完，得到的就是答案，如果是欧拉路径得到的必然通向结束点，中间可能会漏掉环，需要遍历出这些环，并并入路径。

> 不管是环还是链路，都可能存在环中环、链中环的复杂情况，通过遍历拼接都可以解决。

```java
class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        List<String> res = new LinkedList<>();
        if (tickets == null || tickets.size() == 0) return res;

        // 结点集
        Set<String> verName = new HashSet<>();
        for (List<String> ticket : tickets) {
            verName.add(ticket.get(0));
            verName.add(ticket.get(1));
        }
        int verNum = verName.size();
        // 根据结点字母序排序，方便后面搜索先搜索排序靠前的结点
        String[] verIdx = verName.toArray(new String[verNum]);
        Arrays.sort(verIdx);
        // 结点名称索引映射
        Map<String, Integer> idxMap = new HashMap<>();
        for (int i = 0; i < verNum; i++) {
            idxMap.put(verIdx[i], i);
        }

        // 边集
        Node[] graph = new Node[verNum];
        for (List<String> ticket : tickets) {
            int start = idxMap.get(ticket.get(0));
            int end = idxMap.get(ticket.get(1));
            // 插入保持索引的有序性
            if (graph[start] == null || graph[start].val >= end) {
                graph[start] = new Node(end, graph[start]);
            } else {
                Node pre = null;
                Node temp = graph[start];
                while (temp != null && temp.val < end) {
                    pre = temp;
                    temp = temp.next;
                }
                pre.next = new Node(end, temp);
            }
        }
        int start = idxMap.get("JFK");
        // 拼接的链表，贪心从start开始遍历（由于遍历的路径是有序的，abba，如果a存在环，要拼接在第二个a上，所以为了方便遍历，采取倒序的形式）
        // 如果是欧拉回路，此处已经遍历完所有的边
        Node head = dfs(start, graph, null);
        // 如果是欧拉路径，则还需倒序遍历当前路径上的结点，如果存在未访问的边，则遍历拼接
        Node temp = head;
        while (temp != null) {
            if (graph[temp.val] != null) {
                // 当前结点temp存在环路，需要遍历拼接，将temp的前一个结点传入拼接（倒序下一个）
                Node newHead = dfs(temp.val, graph, temp.next);
                // 遍历得到的必然是子环，如果不是环，则图不合法不存在欧拉路径或回路
                if (newHead.val != temp.val) throw new IllegalArgumentException("error graph");
                // 拼接，由于temp结点会和newHead结点重复，选择temp和head的后继拼接
                // 当前点xxx，返回链表为xxx->...->xxx，重复
                temp.next = newHead.next;
            }
            temp = temp.next;
        }
        // 倒序插入
        while (head != null) {
            res.add(0, verIdx[head.val]);
            head = head.next;
        }
        return res;
    }

    private Node dfs(int start, Node[] graph, Node head) {
        // 拼接入路径（倒序插入）顺序后面的接在前面
        head = new Node(start, head);
        // 无节点可遍历
        if (graph[start] == null) return head;

        // 由于已排序，当前结点字母序是最小的，贪心遍历
        Node cur = graph[start];
        // 删除当前点
        graph[start] = graph[start].next;
        // 移动到后面
        return dfs(cur.val, graph, head);
    }
}

class Node {
    int val;
    Node next;

    Node(int val, Node next) {
        this.val = val;
        this.next = next;
    }
}
```

## 性能分析

&emsp;暴力回溯法，邻接矩阵时间复杂度为$O(V^2M)$，空间复杂度为$O(V^2)$，$M$为回溯次数。邻接表时间复杂度为$O(EM)$，空间复杂度为$O(V + E)$。

执行用时：11ms，在所有java提交中击败了48.71%的用户。

内存消耗：42.1MB，在所有java提交中击败了60.92%的用户。

&emsp;欧拉回路拼接法时间复杂度为$O(V\log_2V + E)$，空间复杂度为$O(V + E)$。

执行用时：10ms，在所有java提交中击败了59.01%的用户。

内存消耗：42.1MB，在所有java提交中击败了59.75%的用户。

## 官方解题

&emsp;暂无，密切关注。社区的思路巧妙利用深度遍历模拟环路的遍历拼接，而不用实际拼接，性能更好。其依据是当前结点选择最优路径遍历，遍历完后如果当前结点还存在后继结点，说明存在环，继续在当前结点遍历环并倒插拼接。可见是上述思路的简化。

```java
class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        List<String> res = new LinkedList<>();
        if (tickets == null || tickets.size() == 0) return res;

        // 使用堆来维护后继结点的有序性
        Map<String, PriorityQueue<String>> graph = new HashMap<>();
        for (List<String> ticket : tickets) {
            String from = ticket.get(0);
            String to = ticket.get(1);
            if(graph.get(from) == null) {
                graph.put(from, new PriorityQueue<>());
            }
            graph.get(from).add(to);
        }

        dfs("JFK", graph, res);
        return res;
    }

    private void dfs(String start, Map<String, PriorityQueue<String>> graph, List<String> res) {
        // 按照结点优先级遍历，遍历完最优路径，如果start点还存在环，则继续遍历环并拼接
        while (graph.get(start) != null && !graph.get(start).isEmpty()) {
            dfs(graph.get(start).poll(), graph, res);
        }
        // 倒插
        res.add(0, start);
    }
}
```

&emsp;社区欧拉回路版本时间复杂度为$O(E\log_2E)$，空间复杂度为$O(V + E)$。

执行用时：7ms，在所有java提交中击败了93.17%的用户。

内存消耗：42.1MB，在所有java提交中击败了59.75%的用户。

&emsp;再次回首之前的解法，可以使用字典保存图，从而省去遍历结点集，构建边集等操作，则欧拉回路拼接法修改如下：

```java
class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        List<String> res = new LinkedList<>();
        if (tickets == null || tickets.size() == 0) return res;

        // 结点集
        Map<String, Node> graph = new HashMap<>();
        // 插入保持有序性
        for (List<String> ticket : tickets) {
            if (graph.get(ticket.get(0)) == null ||graph.get(ticket.get(0)).val.compareTo(ticket.get(1)) >= 0 ) {
                graph.put(ticket.get(0), new Node(ticket.get(1), graph.get(ticket.get(0))));
            } else {
                Node pre = null;
                Node temp = graph.get(ticket.get(0));
                while (temp != null && temp.val.compareTo(ticket.get(1)) < 0) {
                    pre = temp;
                    temp = temp.next;
                }
                pre.next = new Node(ticket.get(1), pre.next);
            }

        }
        // 拼接的链表，贪心从start开始遍历（由于遍历的路径是有序的，abba，如果a存在环，要拼接在第二个a上，所以为了方便遍历，采取倒序的形式）
        // 如果是欧拉回路，此处已经遍历完所有的边
        Node head = dfs("JFK", graph, null);
        // 如果是欧拉路径，则还需倒序遍历当前路径上的结点，如果存在未访问的边，则遍历拼接
        Node temp = head;
        while (temp != null) {
            if (graph.get(temp.val) != null) {
                // 当前结点temp存在环路，需要遍历拼接，将temp的前一个结点传入拼接（倒序下一个）
                Node newHead = dfs(temp.val, graph, temp.next);
                // 遍历得到的必然是子环，如果不是环，则图不合法不存在欧拉路径或回路
                if (!newHead.val.equals(temp.val)) throw new IllegalArgumentException("error graph");
                // 拼接，由于temp结点会和newHead结点重复，选择temp和head的后继拼接
                // 当前点xxx，返回链表为xxx->...->xxx，重复
                temp.next = newHead.next;
            }
            temp = temp.next;
        }
        // 倒序插入
        while (head != null) {
            res.add(0, head.val);
            head = head.next;
        }
        return res;
    }

    private Node dfs(String start, Map<String, Node> graph, Node head) {
        // 拼接入路径（倒序插入）顺序后面的接在前面
        head = new Node(start, head);
        // 无节点可遍历
        if (graph.get(start) == null) return head;

        // 由于已排序，当前结点字母序是最小的，贪心遍历
        Node cur = graph.get(start);
        // 删除当前点
        graph.put(start, graph.get(start).next);
        // 移动到后面
        return dfs(cur.val, graph, head);
    }
}

class Node {
    String val;
    Node next;
}
```

