[toc]

Design a logger system that receive stream of messages along with its timestamps, each message should be printed if and only if it is **not printed in the last 10 seconds**.

Given a message and a timestamp (in seconds granularity), return true if the message should be printed in the given timestamp, otherwise returns false.

It is possible that several messages arrive roughly at the same time.



## 题目解读

&emsp;设计日志记录，判断一条日志是否可以打印。打印的规则为同样内容的日志在10秒内不能再次打印。

```java
class Logger {

    /** Initialize your data structure here. */
    public Logger() {

    }
    
    /** Returns true if the message should be printed in the given timestamp, otherwise returns false.
        If this method returns false, the message will not be printed.
        The timestamp is in seconds granularity. */
    public boolean shouldPrintMessage(int timestamp, String message) {

    }
}

/**
 * Your Logger object will be instantiated and called as such:
 * Logger obj = new Logger();
 * boolean param_1 = obj.shouldPrintMessage(timestamp,message);
 */
```

## 程序设计

* 使用哈希表记录日志内容和上次打印时间，如果这次不超过10秒，则不能打印。

```java
class Logger {
    // 记录日志和第一次打印时间
    Map<String, Integer> logRecord;

    /** Initialize your data structure here. */
    public Logger() {
        this.logRecord = new HashMap<>();
    }
    
    public boolean shouldPrintMessage(int timestamp, String message) {
        // 未打印，或时间超过10秒，则可以打印
        if (logRecord.get(message) == null || timestamp - logRecord.get(message) >= 10) {
                logRecord.put(message, timestamp);
            return true;
        }
        return false;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(N)$，其中$N$为日志数。

执行用时：40ms，在所有java提交中击败了55.60%的用户。

内存消耗：51.8MB，在所有java提交中击败了100.00%的用户。


## 官方解题

&emsp;同上。