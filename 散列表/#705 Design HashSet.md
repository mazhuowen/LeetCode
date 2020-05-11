[toc]

Design a `HashSet` without using any built-in hash table libraries.

To be specific, your design should include these functions:

* `add(value)`: Insert a value into the `HashSet`. 
* `contains(value) `: Return whether the value exists in the `HashSet` or not.
* `remove(value)`: Remove a value in the `HashSet`. If the value does not exist in the `HashSet`, do nothing.



Note:

* All values will be in the range of `[0, 1000000]`.
* The number of operations will be in the range of `[1, 10000]`.
* Please do not use the built-in `HashSet` library.



## 题目解读

&emsp;设计集合。

```java
class MyHashSet {

    /** Initialize your data structure here. */
    public MyHashSet() {

    }
    
    public void add(int key) {

    }
    
    public void remove(int key) {

    }
    
    /** Returns true if this set contains the specified element */
    public boolean contains(int key) {

    }
}

/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet obj = new MyHashSet();
 * obj.add(key);
 * obj.remove(key);
 * boolean param_3 = obj.contains(key);
 */
```

##  程序设计

* 不考虑重散列和扩容，开散列表法（官方建议数组长度为质数如769）。

```java
class MyHashSet {
    Node[] set;
    int size;

    /** Initialize your data structure here. */
    public MyHashSet() {
        this.size = 1_000;
        this.set = new Node[size];
    }
    
    public void add(int key) {
        // 已存在
        if (get(key) != null) return;
        // 插入
        set[key % size] = new Node(key, set[key % size]);
    }
    
    public void remove(int key) {
        // 不存在
        if (set[key % size] == null) return;

        Node temp = set[key % size];
        Node pre = null;
        while (temp != null && temp.val != key) {
            pre = temp;
            temp = temp.next;
        }
        // 不存在
        if (temp == null) return;
        if (pre == null) set[key % size] = set[key % size].next;
        else pre.next = temp.next;
    }
    
    /** Returns true if this set contains the specified element */
    public boolean contains(int key) {
        return get(key) != null;
    }

    private Node get(int key) {
        if (set[key % size] == null) return null;
        Node temp = set[key % size];
        while (temp != null && temp.val != key) temp = temp.next;

        return temp;
    }
}

class Node {
    int val;
    Node next;

    Node(int val, Node next) {
        this.val = val;
        this.next = next;
    }
}
```

## 性能分析

&emsp;时间复杂度取决于哈希碰撞链表的长度。

执行用时：19ms，在所有java提交中击败了86.95%的用户。

内存消耗：46.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方除了上述思路，还提供了数组保存二叉树的思路。