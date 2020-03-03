[toc]

Design and implement a TwoSum class. It should support the following operations: `add` and `find`.

`add` - Add the number to an internal data structure.
`find` - Find if there exists any pair of numbers which sum is equal to the value.



## 题目解读

&emsp;设计数据结构支持动态查找两数之和。

```java
class TwoSum {

    /** Initialize your data structure here. */
    public TwoSum() {

    }
    
    /** Add the number to an internal data structure.. */
    public void add(int number) {

    }
    
    /** Find if there exists any pair of numbers which sum is equal to the value. */
    public boolean find(int value) {

    }
}

/**
 * Your TwoSum object will be instantiated and called as such:
 * TwoSum obj = new TwoSum();
 * obj.add(number);
 * boolean param_2 = obj.find(value);
 */
```

## 程序设计

* 最容易想到的是，首先需要保存目前已有的数字，其次需要保存字典维护两数之和。但是这个思路需要在插入数字的时候遍历所有已存在数字并求和，时间复杂度较高。
* 实际上由于是两数之和，且查询调用次数远小于插入方法，可以用哈希表记录数字及其数目，在查询时再遍历求和。

```java
class TwoSum {
    Map<Integer, Integer> record;

    /** Initialize your data structure here. */
    public TwoSum() {
        record = new HashMap<>();
    }
    
    /** Add the number to an internal data structure.. */
    public void add(int number) {
        // 计数加一
        record.put(number, record.getOrDefault(number, 0) + 1);
    }
    
    /** Find if there exists any pair of numbers which sum is equal to the value. */
    public boolean find(int value) {
        // 遍历求和
        for(Map.Entry<Integer, Integer> entry : record.entrySet()) {
            // 判断另一个数是不是其本身
            int complement = value - entry.getKey();
            if (complement != entry.getKey()) {
                if (record.containsKey(complement))
                    return true;
            } else {
                if (entry.getValue() > 1)
                    return true;
            }
        }
        return false;
    }
}
```

## 性能分析

&emsp;插入方法时间复杂度为$O(1)$，查询方法时间复杂度为$O(N)$；空间复杂度为$O(N)$。

执行用时：131ms，在所有java提交中击败了47.41%的用户。

内存消耗：49.6MB，在所有java提交中击败了55.36%的用户。

## 官方解题

&emsp;同上。