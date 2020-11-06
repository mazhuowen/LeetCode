[toc]

Design and implement a data structure for `Least Frequently Used (LFU)` cache. It should support the following operations: `get` and `put`.

* `get(key)` - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
* `put(key, value)` - Set or insert the value if the key is not already present. When the cache reaches its capacity, it should invalidate the least frequently used item before inserting a new item. For the purpose of this problem, when there is a tie (i.e., two or more keys that have the same frequency), the least **recently** used key would be evicted.

Note that the number of times an item is used is the number of calls to the `get` and `put` functions for that item since it was inserted. This number is set to zero when the item is removed.

 

**Follow up**:
Could you do both operations in $O(1)$ time complexity?



## 题目解读

&emsp;设计最不常使用策略，时间复杂度为常量级。

```java
class LFUCache {

    public LFUCache(int capacity) {

    }
    
    public int get(int key) {

    }
    
    public void put(int key, int value) {

    }
}

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache obj = new LFUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

## 程序设计

* 使用哈希表保存使用计数和相应的链表，最久未使用的在链表尾，每次使用（`get`或`put`）将结点移动到新的计数链表的首部。使用另一个哈希表保存键值和链表结点的对应关系，由于是单链表，保存前驱。
* 还需要记录最少的计数，如果当前最少计数链表被删除（由于`get`或`put`旧值原因计数更新），则最少计数加1；如果是`put`新值则最小计数为1。

```java
class LFUCache {
    int capacity;
    // 记录最少计数
    int minCount = 0;
    // 记录使用次数链表，链表末尾为最久未使用
    Map<Integer, Node> head;
    Map<Integer, Node> tail;
    // 记录键值在次数表中的索引
    Map<Integer, Pair> idxMap;

    public LFUCache(int capacity) {
        this.capacity = capacity;
        this.head = new HashMap<>();
        this.tail = new HashMap<>();
        this.idxMap = new HashMap<>();
    }

    public int get(int key) {
        Pair p = idxMap.get(key);
        // 不存在
        if (p == null) return -1;
        // 更新计数
        int count = p.count;
        Node pre = p.pre, cur = pre.next;
        if (cur == null) throw new RuntimeException("error logic");

        // 在原计数删除
        remove(count, pre, cur);

        // 加入新计数
        insert(count + 1, cur, p);
        return cur.value;
    }

    public void put(int key, int value) {
        // 存在则覆盖更新
        if (get(key) != -1) {
            idxMap.get(key).pre.next.value = value;
            return;
        }
        // 需要移除最少次数最久未使用结点
        if (idxMap.size() == capacity) {
            Node t = tail.get(minCount);
            // 待删除点所在链表为空，capacity为0造成，直接返回
            if (t == head.get(minCount)) return;
            Node pre = idxMap.get(t.key).pre;
            remove(minCount, pre, t);
            idxMap.remove(t.key);
        }
        // 直接插入，此时计数最小为1
        minCount = 1;
        Node cur = new Node(key, value, 1);
        Pair p = new Pair(1, null);
        insert(1, cur, p);
        idxMap.put(key, p);
    }

    private void insert(int count, Node cur, Pair p) {
        // 更新计数
        cur.count = count;
        p.count = count;
        // 哑结点
        if (head.get(count) == null) head.put(count, new Node(-1, -1, -1));
        // 头插法
        cur.next = head.get(count).next;
        head.get(count).next = cur;
        // 更新前驱关系
        if (cur.next != null) idxMap.get(cur.next.key).pre = cur;
        else tail.put(count, cur);
        p.pre = head.get(count);
    }

    private void remove(int count, Node pre, Node cur) {
        // 在原计数删除cur
        pre.next = cur.next;
        // 更新尾结点或更新前驱关系
        if (pre.next == null) tail.put(count, pre);
        else idxMap.get(pre.next.key).pre = pre;

        // 判断删除是否导致最小计数发生变化
        if (minCount == count && head.get(count) == tail.get(count)) minCount++;
    }
}

class Node {
    int key;
    int value;
    int count;

    Node next;

    public Node(int key, int value, int count) {
        this.key = key;
        this.value = value;
        this.count = count;
    }

    public Node(int key, int value, int count, Node next) {
        this.key = key;
        this.value = value;
        this.count = count;
        this.next = next;
    }
}

class Pair {
    int count;
    // 记录前驱结点，方便删除
    Node pre;

    public Pair(int count, Node pre) {
        this.count = count;
        this.pre = pre;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$，空间复杂度为$O(N)$。

执行用时：25ms，在所有java提交中击败了73.77%的用户。

内存消耗：47.7MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;上述参考官方思路。社区使用双向链表，结构和代码复杂度均下降。