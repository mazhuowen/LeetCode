[toc]

On a **single threaded** CPU, we execute some functions.  Each function has a unique id between 0 and N-1.

We store logs in timestamp order that describe when a function is entered or exited.

Each log is a string with this format: "`{function_id}:{"start" | "end"}:{timestamp}`".  For example, "`0:start:3`" means the function with id `0` **started at the beginning** of timestamp `3`.  "`1:end:2`" means the function with id `1` ended at the end of timestamp `2`.

A function's exclusive time is the number of units of time spent in this function.  Note that this does **not** include any recursive calls to child functions.

The CPU is **single threaded** which means that only one function is being executed at a given time unit.

Return the exclusive time of each function, sorted by their function id.



**Note**:

* $1 \le n \le 100$
* Two functions won't start or end at the same time.
* Functions will always log when they exit.



## 题目解读

&emsp;题目给定的日志是按照时间戳排列，根据日志求得每个方法调用的时间。需要注意允许递归调用。

```java
class Solution {
    public int[] exclusiveTime(int n, List<String> logs) {
        
    }
}
```

## 程序设计

* 考虑：`0:start:0`，`1:start:2`，`1:end:4`，`2:start:6`，`2:end:7`，`0:end:8`，方法1费时3，方法2费时2很容易计算得出，但方法0比较难计算，首先`0~1`运行2个时间戳，`2~0`运行1个时间戳，除了首尾之外，在中间方法1调用完成而方法2开始前，方法0运行1个时间戳，最后方法0共花费4个时间戳。
* 由上面看到最外层调用的时间计算比较复杂，可以引入栈来模拟记录函数的调用。

```java
class Solution {
    public int[] exclusiveTime(int n, List<String> logs) {
        int[] res = new int[n];
        // 栈保存方法id
        Stack<Integer> stack = new Stack<>();
        Integer pre = null;
        for(String log : logs) {
            String[] info = log.split(":");
            if(stack.isEmpty()) {
                stack.push(Integer.valueOf(info[0]));
                // 更新前一时刻值
                pre = Integer.valueOf(info[2]);
            } else {
                // 方法调用开始
                if("start".equals(info[1])) {
                    // 栈顶是父调用，计算父调用时间
                    // 此时pre为上一个子调用结束的时间或父调用开始的时间
                    // 总之上个子调用结束到现在开始或父调用开始到现在开始都属于父调用时间，更新
                    res[stack.peek()] += Integer.valueOf(info[2]) - pre;
                    // 子调用入栈
                    stack.push(Integer.valueOf(info[0]));
                    // 更新前一时刻值
                    pre = Integer.valueOf(info[2]);
                } 
                // 方法调用结束，计算调用时间
                else {
                    // 方法调用结束，弹出栈并更新时间
                    // 此时pre是子调用结束时间或父调用结束时间
                    res[stack.pop()] += Integer.valueOf(info[2]) - pre + 1;
                    // 更新前一时刻值（由于单线程，运行时间为当前的下一刻）
                    pre = Integer.valueOf(info[2]) + 1;
                }
            }
        }
        return res;
    }
}
```

## 性能分析

&emsp;时间复杂度$O(N)$，空间复杂度$O(N)$。

执行用时：22ms，在所有java提交中击败了56.44%的用户。

内存消耗：38.4MB，在所有java提交中击败了44.25%的用户。

## 官方解题

&emsp;思路同上。