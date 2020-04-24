[toc]

Design a data structure that supports all following operations in average $O(1)$ time.

* `insert(val)`: Inserts an item val to the set if not already present.
* `remove(val)`: Removes an item val from the set if present.
* `getRandom`: Returns a random element from current set of elements. Each element must have the same probability of being returned.



## 题目解读

&emsp;设计支持随机访问的集合，时间复杂度为$O(1)$。

```java
class RandomizedSet {

    /** Initialize your data structure here. */
    public RandomizedSet() {

    }
    
    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    public boolean insert(int val) {

    }
    
    /** Removes a value from the set. Returns true if the set contained the specified element. */
    public boolean remove(int val) {

    }
    
    /** Get a random element from the set. */
    public int getRandom() {

    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```

## 程序设计

* 官方解题使用哈希表保存数值和在链表中的索引。严格意义上，操作不是$O(1)$的。

```java
class RandomizedSet {
  Map<Integer, Integer> dict;
  List<Integer> list;
  Random rand = new Random();

  public RandomizedSet() {
    dict = new HashMap();
    list = new ArrayList();
  }

  public boolean insert(int val) {
    if (dict.containsKey(val)) return false;

    dict.put(val, list.size());
    list.add(list.size(), val);
    return true;
  }

  public boolean remove(int val) {
    if (! dict.containsKey(val)) return false;

    int lastElement = list.get(list.size() - 1);
    int idx = dict.get(val);
    list.set(idx, lastElement);
    dict.put(lastElement, idx);
    list.remove(list.size() - 1);
    dict.remove(val);
    return true;
  }

  public int getRandom() {
    return list.get(rand.nextInt(list.size()));
  }
}
```

## 性能分析

&emsp;大部分情况时间复杂度为$O(1)$，空间复杂度为$O(N)$。

执行用时：13ms，在所有java提交中击败了78.61%的用户。

内存消耗：45.1MB，在所有java提交中击败了71.43%的用户。

## 官方解题

&emsp;见上。