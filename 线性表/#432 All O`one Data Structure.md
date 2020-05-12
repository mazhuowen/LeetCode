[toc]

Implement a data structure supporting the following operations:

* `Inc(Key)` - Inserts a new key with value 1. Or increments an existing key by 1. Key is guaranteed to be a non-empty string.
* `Dec(Key)` - If Key's value is 1, remove it from the data structure. Otherwise decrements an existing key by 1. If the key does not exist, this function does nothing. Key is guaranteed to be a non-empty string.
* `GetMaxKey()` - Returns one of the keys with maximal value. If no element exists, return an empty string `""`.
* `GetMinKey()` - Returns one of the keys with minimal value. If no element exists, return an empty string `""`.

Challenge: Perform all these in $O(1)$ time complexity.



## 题目解读

&emsp;实现时间复杂度为$O(1)$的字符串计数器。

```java
class AllOne {

    /** Initialize your data structure here. */
    public AllOne() {

    }
    
    /** Inserts a new key <Key> with value 1. Or increments an existing key by 1. */
    public void inc(String key) {

    }
    
    /** Decrements an existing key by 1. If Key's value is 1, remove it from the data structure. */
    public void dec(String key) {

    }
    
    /** Returns one of the keys with maximal value. */
    public String getMaxKey() {

    }
    
    /** Returns one of the keys with Minimal value. */
    public String getMinKey() {

    }
}

/**
 * Your AllOne object will be instantiated and called as such:
 * AllOne obj = new AllOne();
 * obj.inc(key);
 * obj.dec(key);
 * String param_3 = obj.getMaxKey();
 * String param_4 = obj.getMinKey();
 */
```

## 程序设计

* 题目要求最大最小计数值只需返回相同计数的任意键值，其次考虑到计数无非增一或减一，改变具有一定的顺序性，可以考虑维护计数的顺序链表；计数由低到高串连在链表上，每个不同的计数结点又是一个链表的起始，相同计数的结点在同一链表上。这样查找最大最小计数键值只需在表头或表尾查找，时间复杂度为$O(1)$。
* 为了使得插入删除也为$O(1)$时间复杂度，可以引入哈希表记录每个键值对应结点，每个计数的起始结点。

```java
class AllOne {
    // 计数链表首尾
    Node head;
    Node tail;
    // 键值结点映射
    Map<String, Node> nodeMap;
    // 计数结点映射
    Map<Integer, Node> counterMap;

    /** Initialize your data structure here. */
    public AllOne() {
        // 首尾标识结点
        this.head = new Node(-1, "");
        this.tail = new Node(-1, "");
        head.right = tail;
        tail.left = head;
        this.nodeMap = new HashMap<>();
        this.counterMap = new HashMap<>();
        // 加入头结点
        counterMap.put(-1, head);
    }
    
    /** Inserts a new key <Key> with value 1. Or increments an existing key by 1. */
    public void inc(String key) {
        // 存在，则计数加一
        if (nodeMap.containsKey(key)) {
            Node cur = nodeMap.get(key);
            // 删除当前结点，返回待插入的左侧结点计数
            int left = remove(cur, false);
            // 计数加一，插入
            cur.count++;
            insert(cur, left);
        } 
        // 不存在，创建
        else {
            Node cur = new Node(1, key);
            nodeMap.put(key, cur);
            // 结点计数为1，左侧为头结点-1
            insert(cur, -1);
        }
    }
    
    /** Decrements an existing key by 1. If Key's value is 1, remove it from the data structure. */
    public void dec(String key) {
        if (nodeMap.containsKey(key)) {
            Node cur = nodeMap.get(key);
            // 删除当前结点，并返回待插入的左侧计数
            int left = remove(cur, true);
            // 计数减一，为0则直接删除
            if (--cur.count == 0) {
                nodeMap.remove(key);
                return;
            }
            // 插入
            insert(cur, left);
        }
    }
    
    /** Returns one of the keys with maximal value. */
    public String getMaxKey() {
        if (head.right == tail) return "";
        // 最大值在尾结点后
        return tail.left.key;
    }
    
    /** Returns one of the keys with Minimal value. */
    public String getMinKey() {
        if (head.right == tail) return "";
        // 最小值在头结点后
        return head.right.key;
    }

    // 删除当前结点，并根据标识返回待插入结点左侧计数
    private int  remove(Node node, boolean dec) {
        // 结点不是计数链表上的结点
        if (node.left == null) {
            // 在结点链删除当前点
            Node pre = node.prev;
            pre.next = node.next;
            if (node.next != null) pre.next.prev = pre;
            // 斩断连接
            node.next = node.prev = null;
            // 如果计数增加，则返回当前计数，计数减少则返回前一计数
            return dec ? counterMap.get(node.count).left.count : node.count;
        } 
        // 结点是计数链表上的结点
        else {
            // 后继成为新的计数链表首结点 
            if (node.next != null) {
                // 连接计数结点
                Node next = node.next;
                node.left.right = next;
                next.left = node.left;
                node.right.left = next;
                next.right = node.right;
                // 断除后继连接关系
                next.prev = null;
                node.next = node.left = node.right = null;
                // 维护计数结点关系
                counterMap.put(node.count, next);
                // 计数增加则返回当前计数，计数减少则返回前一计数
                return dec ? next.left.count : next.count;
            } 
            // 删除该计数结点
            else {
                // 记录前一计数
                int leftCount = node.left.count;
                // 在计数链表删除
                node.left.right = node.right;
                node.right.left = node.left;
                node.left = node.right = null;
                // 维护关系
                counterMap.remove(node.count);
                // 返回左侧计数
                return leftCount;
            }
        }
    }

    // 插入当前结点
    private void insert(Node node, int leftCount) {
        // 当前结点计数存在，只需头插法插入链表后
        if (counterMap.containsKey(node.count)) {
            Node pre = counterMap.get(node.count);
            node.next = pre.next;
            if (node.next != null) node.next.prev = node;
            node.prev = pre;
            pre.next = node;
        } 
        // 不存在，则插入左侧结点后
        else {
            Node left = counterMap.get(leftCount);
            node.right = left.right;
            left.right.left = node;
            left.right = node;
            node.left = left;
            counterMap.put(node.count, node);
        }
    }
}

class Node {
    // 计数值
    int count;
    // 键值
    String key;

    // 左右计数链表
    Node left;
    Node right;
    // 双向链表
    Node prev;
    Node next;

    Node(int count, String key) {
        this.count = count;
        this.key = key;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(1)$。

执行用时：22ms，在所有java提交中击败了85.52%的用户。

内存消耗：45.9MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;暂无，一切关注。