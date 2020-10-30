[toc]

Design and implement a data structure for `Least Recently Used (LRU) cache`. It should support the following operations: `get` and `put`.

* `get(key)` - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
* `put(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

The cache is initialized with a **positive** capacity.



**Follow up**:
Could you do both operations in $O(1)$ time complexity?



## 题目解读

&emsp;设计`LRU`。

```java
class LRUCache {

    public LRUCache(int capacity) {

    }
    
    public int get(int key) {

    }
    
    public void put(int key, int value) {

    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

## 程序设计

* 使用链表的方案来实现，`LRU`每次访问都需要把访问元素放到最前面。如果达到规定的容量，插入则删除链表尾结点。
* 由于题目要求时间复杂度为$O(1)$，而链表的插入删除都涉及到遍历，时间复杂度为$O(N)$，如果有额外的数据结构保存每个结点的前驱结点，则可节省遍历的花销。此处采用哈希表保存每个键值的前驱结点。

```java
class LRUCache {
    // 记录元素数目
    private int size;
    private int capacity;
    // 记录头尾结点
    private Node head;
    private Node tail;
    // 记录键值与结点前驱的关系
    private Map<Integer, Node> record;

    public LRUCache(int capacity) {
        this.size = 0;
        this.capacity = capacity;
        // 哑结点
        this.head = new Node(-1, -1, null);
        this.tail = head;
        this.record = new HashMap<>();
    }
    
    public int get(int key) {
        // 不存在
        if (record.get(key) == null) return -1;
        
        Node pre = record.get(key);
        Node cur = pre.next;
        // 只有一个元素，不需要操作，返回值即可
        if (size == 1) return cur.val;

        // 更新尾结点
        if (cur.next == null) tail = pre;

        // 删除放入头结点后
        pre.next = cur.next;
        cur.next = head.next;
        head.next = cur;

        // 更新字典关系
        record.put(key, head);
        record.put(cur.next.key, cur);
        if (pre.next != null) record.put(pre.next.key, pre);
        return cur.val;
    }
    
    public void put(int key, int value) {
        // 原先就有值，删除
        if (record.get(key) != null) {
            Node pre = record.get(key);
            pre.next = pre.next.next;
            if (pre.next == null) tail = pre;
            else record.put(pre.next.key, pre);
            record.remove(key);
            size--;
        }
        // 需要删除尾结点
        if (size == capacity) {
            int k = tail.key;
            tail = record.get(k);
            tail.next = null;
            record.remove(k);
            size--;
        }

        Node cur = new Node(key, value, head.next);
        head.next = cur;
        record.put(key, head);
        if (cur.next != null) record.put(cur.next.key, cur);
        else tail = cur;
        size++;
    }
}

class Node {
    int key;
    int val;
    Node next;

    Node(int key, int val, Node next) {
        this.key = key;
        this.val = val;
        this.next = next;
    }
}
```

* 优化代码结构如下：

```java
class LRUCache {
    // 记录元素数目
    private int size;
    private int capacity;
    // 记录头尾结点
    private Node head;
    private Node tail;
    // 记录键值与结点前驱的关系
    private Map<Integer, Node> record;

    public LRUCache(int capacity) {
        this.size = 0;
        this.capacity = capacity;
        // 哑结点
        this.head = new Node(-1, -1, null);
        this.tail = head;
        this.record = new HashMap<>();
    }
    
    public int get(int key) {
        // 不存在
        if (record.get(key) == null) return -1;
        
        Node pre = record.get(key);
        Node cur = pre.next;

        // 删除当前结点
        pre.next = cur.next;
        // 更新尾结点或更新前驱关系
        if (pre.next == null) tail = pre;
        else record.put(pre.next.key, pre);

        // 插入头结点之后
        cur.next = head.next;
        head.next = cur;
        // 更新字典关系
        record.put(key, head);
        if (cur.next == null) tail = cur;
        else record.put(cur.next.key, cur);

        return cur.val;
    }
    
    public void put(int key, int value) {
        // 原先就有值，覆盖更新并返回（查找操作会放入头结点之后）
        if (get(key) != -1) {
            head.next.val = value;
            return;
        }
        // 需要删除尾结点
        if (size == capacity) {
            int k = tail.key;
            tail = record.get(k);
            tail.next = null;
            // 删除关系
            record.remove(k);
            size--;
        }

        // 加入新结点
        Node cur = new Node(key, value, head.next);
        head.next = cur;
        record.put(key, head);
        if (cur.next == null) tail = cur;
        else record.put(cur.next.key, cur);
        size++;
    }
}

class Node {
    int key;
    int val;
    Node next;

    Node(int key, int val, Node next) {
        this.key = key;
        this.val = val;
        this.next = next;
    }
}
```

## 性能分析

&emsp;构建、插入、查找时间复杂度均为$O(1)$，空间复杂度为$O(N)$。

执行用时：22ms，在所有java提交中击败了59.75%的用户。

内存消耗：48.2MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方思路为采用`LinkedHashMap`，通过实现`removeEldestEntry`方法来实现`LRU`功能，这是java官方的接口。

```java
class LRUCache extends LinkedHashMap<Integer, Integer>{
    private int capacity;
    
    public LRUCache(int capacity) {
        super(capacity, 0.75F, true);
        this.capacity = capacity;
    }

    public int get(int key) {
        return super.getOrDefault(key, -1);
    }

    public void put(int key, int value) {
        super.put(key, value);
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity; 
    }
}
```

&emsp;时间复杂度同上。

执行用时：30ms，在所有java提交中击败了33.82%的用户。

内存消耗：48.3MB，在所有java提交中击败了100.00%的用户。

&emsp;官方还提供了链表加哈希表的思路，不同于上面，元素是尾插法，这样最长时间未使用的元素就在链表头。社区再多的采用双向链表，查询更方便。