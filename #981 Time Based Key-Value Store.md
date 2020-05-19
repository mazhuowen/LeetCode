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

&emsp;

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

* 

```java

```

## 性能分析

&emsp;



## 官方解题

&emsp;