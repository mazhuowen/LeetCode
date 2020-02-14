[toc]

Design your implementation of the linked list. You can choose to use the singly linked list or the doubly linked list. A node in a singly linked list should have two attributes: `val` and `next. val` is the value of the current node, and `next` is a pointer/reference to the next node. If you want to use the doubly linked list, you will need one more attribute `prev` to indicate the previous node in the linked list. Assume all nodes in the linked list are 0-indexed.

Implement these functions in your linked list class:

* `get(index)` : Get the value of the `index`-th node in the linked list. If the index is invalid, return -1.
* `addAtHead(val)` : Add a node of value `val` before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
* `addAtTail(val)` : Append a node of value `val` to the last element of the linked list.
* `addAtIndex(index, val)` : Add a node of value `val` before the `index`-th node in the linked list. If `index` equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted.
* `deleteAtIndex(index)` : Delete the `index`-th node in the linked list, if the index is valid.



Constraints:

* $0 \le \text{index,val} \le 1000$
* Please do not use the built-in LinkedList library.
* At most 2000 calls will be made to get, addAtHead, addAtTail,  addAtIndex and deleteAtIndex.



## 题目解读

&emsp;选择单链表或双链表设计一个链表类。

```java
class MyLinkedList {

    /** Initialize your data structure here. */
    public MyLinkedList() {
        
    }
    
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    public int get(int index) {
        
    }
    
    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    public void addAtHead(int val) {
        
    }
    
    /** Append a node of value val to the last element of the linked list. */
    public void addAtTail(int val) {
        
    }
    
    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    public void addAtIndex(int index, int val) {
        
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    public void deleteAtIndex(int index) {
        
    }
}

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
```

## 程序设计

* 链表类中需要记录当前结点数，及链表头尾结点。需注意删除插入操作中尾结点的更新。

```java
class MyLinkedList {
    // 记录表头表尾
    private Node head;
    private Node tail;
    // 记录结点数
    private int size;

    class Node {
        int val;
        Node next;

        Node(int val) {
            this.val = val;
        }

        Node(int val, Node next) {
            this.val = val;
            this.next = next;
        }
    }

    /** Initialize your data structure here. */
    public MyLinkedList() {
        // 初始化哑结点
        this.head = new Node(-1);
        this.tail = head;
        this.size = 0;
    }
    
    /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
    public int get(int index) {
        // 非法索引
        if(index >= size || index < 0) {
            return -1;
        }
        // 遍历
        Node traverse = head;
        while(index-- >= 0) {
            traverse = traverse.next;
        }
        return traverse.val;
    }
    
    /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
    public void addAtHead(int val) {
        Node temp = new Node(val, head.next);
        head.next = temp;
        size++;
        // 更新尾结点
        if(temp.next == null) {
            tail = temp;
        }
    }
    
    /** Append a node of value val to the last element of the linked list. */
    public void addAtTail(int val) {
        tail.next = new Node(val);
        size++;
        tail = tail.next;
    }
    
    /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
    public void addAtIndex(int index, int val) {
        if(index < size && index > 0) {
            // 寻找前驱结点
            Node traverse = head;
            while(--index >= 0) {
                traverse = traverse.next;
            }
            // 插入
            Node temp = new Node(val, traverse.next);
            traverse.next = temp;
            size++;
        } else if(index == size) {
            addAtTail(val);
        } else if(index == 0) {
            addAtHead(val);
        }
    }
    
    /** Delete the index-th node in the linked list, if the index is valid. */
    public void deleteAtIndex(int index) {
        if(index >= size || index < 0) {
            return;
        }
        
        // 寻找前驱结点
        Node traverse = head;
        while(--index >= 0) {
            traverse = traverse.next;
        }
        // 更新tail
        if(traverse.next.next == null) {
            tail = traverse;
        }
        // 删除
        traverse.next = traverse.next.next;
        size--;
    }
}
```

## 性能分析

&emsp;插入头尾方法时间复杂度为$O(1)$，根据索引获取、插入、删除是时间复杂度为$O(N)$。整体空间复杂度为$O(N)$。

执行用时：15ms，在所有java提交中击败了56.39%的用户。

内存消耗：53.7MB，在所有java提交中击败了5.12%的用户。

## 官方解题

&emsp;官方还提供了双链表的思路。总体思路一致。