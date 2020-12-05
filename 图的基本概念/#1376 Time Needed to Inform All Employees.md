[toc]

A company has $n$ employees with a unique ID for each employee from $0$ to $n - 1$. The head of the company has is the one with `headID`.

Each employee has one direct manager given in the `manager` array where `manager[i]` is the direct manager of the `i-th` employee, `manager[headID] = -1`. Also it's guaranteed that the subordination relationships have a tree structure.

The head of the company wants to inform all the employees of the company of an urgent piece of news. He will inform his direct subordinates and they will inform their subordinates and so on until all employees know about the urgent news.

The `i-th` employee needs `informTime[i]` minutes to inform all of his direct subordinates (i.e After `informTime[i]` minutes, all his direct subordinates can start spreading the news).

Return the number of minutes needed to inform all the employees about the urgent news.

 

**Example 1**:

```
Input: n = 1, headID = 0, manager = [-1], informTime = [0]
Output: 0
Explanation: The head of the company is the only employee in the company.
```

**Example 2**:

<img src="..\images\#1376_exp1.png"  />

```
Input: n = 6, headID = 2, manager = [2,2,-1,2,2,2], informTime = [0,0,1,0,0,0]
Output: 1
Explanation: The head of the company with id = 2 is the direct manager of all the employees in the company and needs 1 minute to inform them all.
The tree structure of the employees in the company is shown.
```

**Example 3**:

<img src="..\images\#1376_exp2.png" style="zoom: 67%;" />

```
Input: n = 7, headID = 6, manager = [1,2,3,4,5,6,-1], informTime = [0,6,5,4,3,2,1]
Output: 21
Explanation: The head has id = 6. He will inform employee with id = 5 in 1 minute.
The employee with id = 5 will inform the employee with id = 4 in 2 minutes.
The employee with id = 4 will inform the employee with id = 3 in 3 minutes.
The employee with id = 3 will inform the employee with id = 2 in 4 minutes.
The employee with id = 2 will inform the employee with id = 1 in 5 minutes.
The employee with id = 1 will inform the employee with id = 0 in 6 minutes.
Needed time = 1 + 2 + 3 + 4 + 5 + 6 = 21.
```

**Example 4**:

```
Input: n = 15, headID = 0, manager = [-1,0,0,1,1,2,2,3,3,4,4,5,5,6,6], informTime = [1,1,1,1,1,1,1,0,0,0,0,0,0,0,0]
Output: 3
Explanation: The first minute the head will inform employees 1 and 2.
The second minute they will inform employees 3, 4, 5 and 6.
The third minute they will inform the rest of employees.
```

**Example 5**:

```
Input: n = 4, headID = 2, manager = [3,3,-1,2], informTime = [0,0,162,914]
Output: 1076
```



**Constraints**:

* $1 \le n \le 10^5$
* $0 \le \text{headID} < n$
* $\text{manager.length} == n$
* $0 \le \text{manager[i]} < n$
* $\text{manager[headID]} == -1$
* $\text{informTime.length} == n$
* $0 \le \text{informTime[i]} \le 1000$
* $\text{informTime[i]} == 0$ if employee $i$ has no subordinates.
* It is **guaranteed** that all the employees can be informed.



## 题目解读

&emsp;给定员工管理树，求通知到底层员工所需时间。

```java
class Solution {
    public int numOfMinutes(int n, int headID, int[] manager, int[] informTime) {

    }
}
```

## 程序设计

* 实际就是计算树的根节点到叶节点的最长路径，构图然后深度优先搜索。

```java
class Solution {
    public int numOfMinutes(int n, int headID, int[] manager, int[] informTime) {
        // 构图
        Node[] graph = new Node[n];
        for (int i = 0; i < manager.length; i++) {
            if (i == headID) continue;
            graph[manager[i]] = new Node(i, graph[manager[i]]);
        }
        return dfs(headID, graph, informTime);
    }

    private int dfs(int ver, Node[] graph, int[] informTime) {
        int res = informTime[ver], max = 0;
        for (Node tmp = graph[ver]; tmp != null; tmp = tmp.next) {
            max = Math.max(max, dfs(tmp.ver, graph, informTime));
        }
        return res + max;
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

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：23 ms, 在所有 Java 提交中击败了85.71%的用户

内存消耗：51.3 MB, 在所有 Java 提交中击败了95.24%的用户。

## 官方解题

&emsp;暂无，密切关注。