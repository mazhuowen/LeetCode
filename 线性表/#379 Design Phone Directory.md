[toc]

Design a Phone Directory which supports the following operations:

* get: Provide a number which is not assigned to anyone.
* check: Check if a number is available or not.
* release: Recycle or release a number.



## 题目解读

&emsp;设计一个电话目录系统，支持分配号码、验证号码是否可分配、释放号码三个功能。号码有最大限制。题目对怎么分配号码没有详细说明，需要与面试官沟通确定。经过个人测试，目前的分配规则为：当历史分配号码不超过最大号码时，递增继续分配；当分配号码达到最大号码限制时，如果最大号码是释放过得，则分配这个最大号码，如果是有效的，则从前面寻找最小的释放号码，分配给这个号码。

```java
class PhoneDirectory {

    /** Initialize your data structure here
        @param maxNumbers - The maximum numbers that can be stored in the phone directory. */
    public PhoneDirectory(int maxNumbers) {
        
    }
    
    /** Provide a number which is not assigned to anyone.
        @return - Return an available number. Return -1 if none is available. */
    public int get() {
        
    }
    
    /** Check if a number is available or not. */
    public boolean check(int number) {
        
    }
    
    /** Recycle or release a number. */
    public void release(int number) {
        
    }
}

/**
 * Your PhoneDirectory object will be instantiated and called as such:
 * PhoneDirectory obj = new PhoneDirectory(maxNumbers);
 * int param_1 = obj.get();
 * boolean param_2 = obj.check(number);
 * obj.release(number);
 */
```

## 程序设计

* 用链表来记录分配过的号码，对于释放的号码不需要删除，将有效标识置为无效即可。则结合上面分配规则可得到代码。

```java
class PhoneDirectory {
    int maxNumbers;
    // 已分配链表的头尾结点
    Node head;
    Node tail;

    public PhoneDirectory(int maxNumbers) {
        this.maxNumbers = maxNumbers;
        this.head = new Node(-1, false, null);
        this.tail = this.head;
    }
    
    /** Provide a number which is not assigned to anyone.
        @return - Return an available number. Return -1 if none is available. */
    public int get() {
        // 尾部可插入
        if(tail.num < maxNumbers - 1) {
            tail.next = new Node(tail.num + 1,  true,null);
            tail = tail.next;
            return tail.num;
        } 
        // 尾部已经是最大值，但是无效，则置为有效，并返回
        else if(!tail.valid) {
            tail.valid = true;
            return tail.num;
        }
        // 尾部是最大值且有效，则遍历中间链表插入
        else {
            Node temp = head.next;
            // 找到第一个无效的点
            while(temp != null && temp.valid) {
                temp = temp.next;
            }
            // 链表已满且都有效，返回-1
            if(temp == null) {
                return -1;
            } else {
                // 修改为有效
                temp.valid = true;
                return temp.num;
            }
        }
    }
    
    /** Check if a number is available or not. */
    public boolean check(int number) {
        // 号码可以申请
        if(number > tail.num && number < maxNumbers) {
            return true;
        }
        // 无效
        if(number >= maxNumbers) {
            return false;
        }
        Node temp = head.next;
        while(temp != null && temp.num != number) {
            temp = temp.next;
        }
        // 查询的点已经在使用，无法再次申请
        if(temp != null && temp.valid) {
            return false;
        } else {
            return true;
        }
    }
    
    /** Recycle or release a number. */
    public void release(int number) {
        // 无效的数字
        if(number > tail.num) {
            return;
        }
        Node temp = head.next;
        while(temp != null && temp.num != number) {
            temp = temp.next;
        }
        if(temp != null) {
            temp.valid = false;
        }
    }
}

// 链表结点
class Node {
    // 值
    int num;
    // 有效标识（是否删除）
    boolean valid;
    // 后继
    Node next;

    Node(int num, boolean valid, Node next) {
        this.num = num;
        this.valid = valid;
        this.next = next;
    }
}
```

测试样例：重点测试尾结点，当尾结点为最大号码时，尾结点有效和无效的情况下测试分配机制。

## 性能分析

&emsp;三个方法在最糟糕的情况下，时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：62ms，在所有java提交中击败了16.67%的用户。

内存消耗：45.5MB，在所有java提交中击败了14.47%的用户。

## 官方解题

&emsp;暂无，密切关系。

> 社区时间性能较高的方法，思路实际上就是分配号码前，先将之前释放过得最小的结点分配，否则才会按照顺序分配。实际上是错的，但能通过所有测试样例。经过测试，样例`["PhoneDirectory","get","get","get","get","release","release","get", "get"]
> [[5],[],[],[],[],[3],[1],[], []]`该方法与预期方法不一致。该方法是错误的。

```java
class PhoneDirectory {
    // 记录是否在在使用
    private boolean[] phoneDir;
    // 记录下一个分配号码
    private Node phoneNum;

    /** Initialize your data structure here
        @param maxNumbers - The maximum numbers that can be stored in the phone directory. */
    public PhoneDirectory(int maxNumbers) {
        phoneDir = new boolean[maxNumbers];
        phoneNum = new Node(-1);
        Node tail = phoneNum;
        // 初始化链表，尾插法
        for (int i = 0; i < maxNumbers; i++) {
            tail.next = new Node(i);
            tail = tail.next;
        }
    }
    
    /** Provide a number which is not assigned to anyone.
        @return - Return an available number. Return -1 if none is available. */
    public int get() {
        // 待分配结点为空
        if (phoneNum.next==null) {
            return -1;
        }
        // 分配结点
        int val = phoneNum.next.val;
        // 待分配指针指向下一个结点
        phoneNum.next = phoneNum.next.next;
        // 分配标识置为true
        phoneDir[val] = true;
        return val;
    }
    
    /** Check if a number is available or not. */
    public boolean check(int number) {
        // 超过最大号码，不可分配
        if(number >= phoneDir.length) {
            return false;
        }
        // 若在使用该号码，则无效不能再分配
        return !phoneDir[number];
    }
    
    /** Recycle or release a number. */
    public void release(int number) {
        if(number >= phoneDir.length) {
            return;
        }
        // 已经释放过或没分配过，返回
        if (!phoneDir[number]) {
            return;
        }
        // 置为释放
        phoneDir[number] = false;
        // 将释放的号码插入到phoneNum后
        Node node = new Node(number);
        node.next = phoneNum.next;
        phoneNum.next = node;
    }
}

class Node {
    int val;
    Node next;
        
    Node(int val) {
        this.val = val;
    }
}
```

执行用时：12ms，在所有java提交中击败了97.06%的用户。

内存消耗：39.2MB，在所有java提交中击败了90.79%的用户。