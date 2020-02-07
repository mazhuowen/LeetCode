[toc]

Given **n** processes, each process has a unique **PID (process id)** and its **PPID (parent process id)**.

Each process only has one parent process, but may have one or more children processes. This is just like a tree structure. Only one process has PPID that is 0, which means this process has no parent process. All the PIDs will be distinct positive integers.

We use two list of integers to represent a list of processes, where the first list contains PID for each process and the second list contains the corresponding PPID.

Now given the two lists, and a PID representing a process you want to kill, return a list of PIDs of processes that will be killed in the end. You should assume that when a process is killed, all its children processes will be killed. No order is required for the final answer.

**Note:**

* The given kill id is guaranteed to be one of the given PIDs.
* $n >= 1$.



## 题目解读

&emsp;删除进程，同时其子进程也会被删除，实际上其结构为树状结构，删除相当于依次寻找其子节点，很容易想到树的广度优先遍历算法。

```java
class Solution {
    public List<Integer> killProcess(List<Integer> pid, List<Integer> ppid, int kill) {
        
    }
}
```

## 程序设计

* 使用队列来存储需要终结的进程号，每次终结进程相当于当前进程出队，将其子进程入队的过程。
* 需要注意时间限制，可以将要终结的结点从两个链表中删除，缩短遍历时间。

```java
class Solution {
    public List<Integer> killProcess(List<Integer> pid, List<Integer> ppid, int kill) {
        List<Integer> res = new LinkedList<>();
        // 待终结进程
       Queue<Integer> queue = new LinkedList<>();
       queue.add(kill);
        while(!queue.isEmpty()) {
            // 终结进程
            int willKill = queue.poll();
            res.add(willKill);
            // 将其子进程加入队列
            for(int i = 0; i < ppid.size(); i++) {
                // 父进程终结，子进程也要终结，入队等待处理
                if(ppid.get(i) == willKill) {
                    queue.add(pid.get(i));
                    // 考虑到时间限制，删除待终结结点
                    ppid.remove(i);
                    pid.remove(i);
                    // 由于删除，下一次遍历位置也需要减一
                    i--;
                }
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;最坏情况下树是一个链表，而要终结的结点是根进程，需要入队$N$次，每次需要遍历链表，时间复杂度为$N^2$。空间复杂度为$O(N)$。

执行用时：756ms，在所有java提交中击败了14.58%的用户。

内存消耗：50.5MB，在所有java提交中击败了79.54%的用户。

## 官方解题

&emsp;官方使用了深度优先搜索的思路，后续改进都是引入字典保存还原树形结构，通过对树的结点删除来实现。社区运行较快的方法使用字典保存父进程索引和对应的子进程列表，终结进程就是从字典中取出子进程列表，遍历子进程，递归调用终结方法。