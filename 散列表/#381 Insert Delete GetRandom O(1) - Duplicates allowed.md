[toc]

Design a data structure that supports all following operations in average $O(1)$ time.

Note: Duplicate elements are allowed.

* `insert(val)`: Inserts an item val to the collection.
* `remove(val)`: Removes an item val from the collection if present.
* `getRandom`: Returns a random element from current collection of elements. The probability of each element being returned is **linearly related** to the number of same value the collection contains.



## 题目解读

&emsp;设计可存在重复值，各项操作时间复杂度为$O(1)$的数据结构。

```java
class RandomizedCollection {

    /** Initialize your data structure here. */
    public RandomizedCollection() {

    }
    
    /** Inserts a value to the collection. Returns true if the collection did not already contain the specified element. */
    public boolean insert(int val) {

    }
    
    /** Removes a value from the collection. Returns true if the collection contained the specified element. */
    public boolean remove(int val) {

    }
    
    /** Get a random element from the collection. */
    public int getRandom() {

    }
}

/**
 * Your RandomizedCollection object will be instantiated and called as such:
 * RandomizedCollection obj = new RandomizedCollection();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```

## 程序设计

* 由于随即采样与数量成正相关，最好将所有元素保存在数组，这样只需随机数生成访问即可；为了使得插入、删除操作时间复杂度为$O(1)$，需要引入哈希表保存每个数值在数组中的索引，这样删除时只需替换数组尾元素到待删除位置，然后删除数组尾即可。

```java
class RandomizedCollection {
    int size;
    Random random;
    List<Integer> list;
    Map<Integer, Set<Integer>> map;

    /** Initialize your data structure here. */
    public RandomizedCollection() {
        this.size = 0;
        this.random = new Random();
        this.list = new ArrayList<>();
        this.map = new HashMap<>();
    }
    
    /** Inserts a value to the collection. Returns true if the collection did not already contain the specified element. */
    public boolean insert(int val) {
        if (map.get(val) == null) map.put(val, new HashSet<>());

        // 新增则添加到数组尾，更新索引关系
        map.get(val).add(size++);
        list.add(val);
        return map.get(val).size() == 1;
    }
    
    /** Removes a value from the collection. Returns true if the collection contained the specified element. */
    public boolean remove(int val) {
        if (map.get(val) == null || map.get(val).isEmpty()) return false;

        // 删除则替换数组尾删除
        int remove = map.get(val).iterator().next();
        map.get(val).remove(remove);

        if (remove != --size) {
            // 将尾部值放入删除位置
            list.set(remove, list.get(size));
            // 维护原尾部值在字典中关系
            map.get(list.get(remove)).remove(size);
            map.get(list.get(remove)).add(remove);
        }
        // 删除尾部
        list.remove(size);
        return true;
    }
    
    /** Get a random element from the collection. */
    public int getRandom() {
        int idx = random.nextInt(size);
        return list.get(idx);
    }
}
```

## 性能分析

&emsp;各项操作时间复杂度为$O(1)$，空间复杂度为$O(N)$。

执行用时：15ms，在所有java提交中击败了88.93%的用户。

内存消耗：46.8MB，在所有java提交中击败了60.00%的用户。

## 官方解题

&emsp;同上。