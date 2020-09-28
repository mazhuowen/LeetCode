[toc]

You are given several logs that each log contains a unique id and timestamp. Timestamp is a string that has the following format: `Year:Month:Day:Hour:Minute:Second`, for example, `2017:01:01:23:59:59`. All domains are zero-padded decimal numbers.

Design a log storage system to implement the following functions:

`void Put(int id, string timestamp)`: Given a log's unique id and timestamp, store the log in your storage system.

`int[] Retrieve(String start, String end, String granularity)`: Return the id of logs whose timestamps are within the range from start to end. Start and end all have the same format as timestamp. However, granularity means the time level for consideration. For example, start = "2017:01:01:23:59:59", end = "2017:01:02:23:59:59", granularity = "Day", it means that we need to find the logs within the range from Jan. 1st 2017 to Jan. 2nd 2017.



**Note**:

* There will be at most $300$ operations of Put or Retrieve.
* Year ranges from `[2000,2017]`. Hour ranges from `[00,23]`.
* Output for Retrieve has no order required.



## 题目解读

&emsp;设计数据结构使得可以返回时间戳区间内得所有日志。

```java
class LogSystem {

    public LogSystem() {

    }
    
    public void put(int id, String timestamp) {

    }
    
    public List<Integer> retrieve(String s, String e, String gra) {

    }
}

/**
 * Your LogSystem object will be instantiated and called as such:
 * LogSystem obj = new LogSystem();
 * obj.put(id,timestamp);
 * List<Integer> param_2 = obj.retrieve(s,e,gra);
 */
```

## 程序设计

* 将时间戳转换为数值，然后存入红黑树。

```java
class LogSystem {
    Map<String, Integer> relation;
    TreeMap<Long, Integer> logSystem;

    public LogSystem() {
        this.relation = new HashMap<>();
        relation.put("Year", 0);
        relation.put("Month", 1);
        relation.put("Day", 2);
        relation.put("Hour", 3);
        relation.put("Minute", 4);
        relation.put("Second", 5);

        this.logSystem = new TreeMap<>();
    }
    
    public void put(int id, String timestamp) {
        logSystem.put(trans2num(timestamp.split(":")), id);
    }
    
    public List<Integer> retrieve(String s, String e, String gra) {
        if (!relation.containsKey(gra)) throw new IllegalArgumentException("invalid param");
        long start = trans2num(cutDate(s, relation.get(gra), false));
        long end = trans2num(cutDate(e, relation.get(gra), true));

        List<Integer> res = new LinkedList<>();

        for (Map.Entry<Long, Integer> entry : logSystem.tailMap(start - 1).entrySet()) {
            if (entry.getKey() > end) break;
            res.add(entry.getValue());
        }
        return res;
    }

    // 将字符串时间戳转换为数值
    private long trans2num(String[] times) {
        long res = 0;
        res += (Integer.parseInt(times[0]) - 1999L) * 12 * 31 * 24 * 60 * 60;
        res += (Integer.parseInt(times[1]) - 1L) * 31 * 24 * 60 * 60;
        res += (Integer.parseInt(times[2]) - 1L) * 24 * 60 * 60;
        res += Integer.parseInt(times[3]) * 60 * 60;
        res += Integer.parseInt(times[4]) * 60;
        res += Integer.parseInt(times[5]);
        return res;
    }

    // 根据upper拼接上限或下限
    private String[] cutDate(String date, int gra, boolean upper) {
        String[] times = date.split(":");
        for (int i = gra + 1; i < times.length; i++) {
            if (upper) {
                if (i == 1) times[i] = "12";
                else if (i == 2) times[i] = "31";
                else if (i == 3) times[i] = "23";
                else if (i == 4) times[i] = "59";
                else if (i == 5) times[i] = "59";
            } else {
                if (i == 1 || i == 2) times[i] = "01";
                else times[i] = "00";
            }
        }
        return times;
    }
}
```

## 性能分析

&emsp;插入时间复杂度为$O(\log_2N)$，区间查找时间复杂度为$O(N)$。

执行用时：47ms，在所有java提交中击败了79.07%的用户。

内存消耗：39.8MB，在所有java提交中击败了67.05%的用户。

## 官方解题

&emsp;官方思路类似。