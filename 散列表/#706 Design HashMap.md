[toc]

Design a HashMap without using any built-in hash table libraries.

To be specific, your design should include these functions:

* `put(key, value)` : Insert a `(key, value)` pair into the HashMap. If the value already exists in the HashMap, update the value.
* `get(key)`: Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key.
* `remove(key)` : Remove the mapping for the value key if this map contains the mapping for the key.



**Note**:

* All keys and values will be in the range of `[0, 1000000]`.
* The number of operations will be in the range of `[1, 10000]`.
* Please do not use the built-in HashMap library.



## 题目解读

&emsp;设计哈希表。

```java
class MyHashMap {

    /** Initialize your data structure here. */
    public MyHashMap() {

    }
    
    /** value will always be non-negative. */
    public void put(int key, int value) {

    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    public int get(int key) {

    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    public void remove(int key) {

    }
}

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap obj = new MyHashMap();
 * obj.put(key,value);
 * int param_2 = obj.get(key);
 * obj.remove(key);
 */
```

## 程序设计

* 设计变长的哈希表，超时，主要花销为重散列时数组拷贝。

```java
class MyHashMap {
    private int size;
    private Node[] table;

    public MyHashMap() {
        this.size = 0;
        this.table = new Node[9];
    }
    
    public void put(int key, int value) {
        if (size > table.length / 2) rehash();

        Node temp = table[getHash(key)];
        while (temp != null && temp.key != key) temp = temp.next;

        if (temp != null) temp.val = value;
        else {
            table[getHash(key)] = new Node(key, value, table[getHash(key)]);
            size++;
        }
    }
    
    public int get(int key) {
        Node temp = table[getHash(key)];
        while (temp != null && temp.key != key) temp = temp.next;

        return temp == null ? -1 : temp.val;
    }
    
    public void remove(int key) {
        Node temp = table[getHash(key)];
        if (temp == null) return;
        if (temp.key == key) {
            table[getHash(key)] = temp.next;
            size--;
            return;
        }
        while (temp.next != null && temp.next.key != key) temp = temp.next;
        if (temp.next != null) {
            temp.next = temp.next.next;
            size--;
        }
    }

    private int getHash(int key) {
        return key % table.length;
    }

    private void rehash() {
        int newSize = size >= Integer.MAX_VALUE / 2 ? Integer.MAX_VALUE : size * 2 + 1;
        Node[] temp = table;
        table = new Node[newSize];
        for (Node cur : temp) {
            if (cur == null) continue;
            while (cur != null) {
                put(cur.key, cur.val);
                cur  = cur.next;
            }
        }
        temp = null;
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
}
```

* 事实上考虑复杂了，不考虑重散列，初始化时指定尺寸为2069：

```java
class MyHashMap {
    private int size;
    private Node[] table;

    public MyHashMap() {
        this.size = 0;
        // 初始指定容量
        this.table = new Node[2069];
    }
    
    public void put(int key, int value) {
        // 本题无需考虑重散列
        // if (size > table.length / 2) rehash();

        Node temp = table[getHash(key)];
        while (temp != null && temp.key != key) temp = temp.next;

        if (temp != null) temp.val = value;
        else {
            table[getHash(key)] = new Node(key, value, table[getHash(key)]);
            size++;
        }
    }
    
    public int get(int key) {
        Node temp = table[getHash(key)];
        while (temp != null && temp.key != key) temp = temp.next;

        return temp == null ? -1 : temp.val;
    }
    
    public void remove(int key) {
        Node temp = table[getHash(key)];
        if (temp == null) return;
        if (temp.key == key) {
            table[getHash(key)] = temp.next;
            size--;
            return;
        }
        while (temp.next != null && temp.next.key != key) temp = temp.next;
        if (temp.next != null) {
            temp.next = temp.next.next;
            size--;
        }
    }

    private int getHash(int key) {
        return key % table.length;
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
}
```

## 性能分析

&emsp;时间复杂度最坏为$O(N)$，空间复杂度为$O(N)$。

执行用时：18ms，在所有java提交中击败了99.72%的用户。

内存消耗：42.8MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;同上。