[toc]

Create a timebased key-value store class `TimeMap`, that supports two operations.

* `set(string key, string value, int timestamp)`
  * Stores the `key` and `value`, along with the given `timestamp`.

* `get(string key, int timestamp)`
  * Returns a value such that `set(key, value, timestamp_prev)` was called previously, with `timestamp_prev <= timestamp`.
  * If there are multiple such values, it returns the one with the largest `timestamp_prev`.
  * If there are no values, it returns the empty string (`""`).



Note:

* All key/value strings are lowercase.
* All key/value strings have length in the range `[1, 100]`
* The `timestamps` for all `TimeMap.set` operations are strictly increasing.
* $1 \le \text{timestamp} \le 10^7$
* `TimeMap.set` and `TimeMap.get` functions will be called a total of `120000` times (combined) per test case.



## 题目解读

&emsp;设计时间辍字典，根据键值和时间戳返回最近的时间辍的值。题目限定时间戳是递增的。

```java
class TimeMap {

    /** Initialize your data structure here. */
    public TimeMap() {

    }
    
    public void set(String key, String value, int timestamp) {

    }
    
    public String get(String key, int timestamp) {

    }
}

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap obj = new TimeMap();
 * obj.set(key,value,timestamp);
 * String param_2 = obj.get(key,timestamp);
 */
```

## 程序设计

* 由于时间戳是递增的，字典可维护有序链表，采用二分查找。

```java
class TimeMap {
    Map<String, List<Pair>> map;

    /** Initialize your data structure here. */
    public TimeMap() {
        this.map = new HashMap<>();
    }
    
    public void set(String key, String value, int timestamp) {
        if (!map.containsKey(key)) map.put(key, new ArrayList<>());
        // 由于时间戳递增，直接放到数组末尾
        map.get(key).add(new Pair(timestamp, value));
    }
    
    public String get(String key, int timestamp) {
        List<Pair> l = map.get(key);
        if (l == null || l.size() == 0) return "";

        // 采用二分查找
        int idx = binarySearch(l, timestamp, 0, l.size() - 1);
        return idx == -1 ? "" : l.get(idx).val;
    }

    private int binarySearch(List<Pair> l, int target, int left, int right) {
        while (left < right) {
            int mid = left + (right - left) / 2 + 1;
            if (l.get(mid).timestamp <= target) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        if (left == 0 && l.get(0).timestamp > target) return -1;
        return left;
    }
}

class Pair {
    int timestamp;
    String val;

    public Pair(int timestamp, String val) {
        this.timestamp = timestamp;
        this.val = val;
    }
}
```

## 性能分析

&emsp;插入时间复杂度为$O(1)$，查找时间复杂度为$O(\log_2N)$；空间复杂度为$O(N)$。

执行用时：175ms，在所有java提交中击败了83.82%的用户。

内存消耗：113.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方提供了红黑树的思路。