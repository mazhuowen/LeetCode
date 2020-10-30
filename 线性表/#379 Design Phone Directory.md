[toc]

Design a Phone Directory which supports the following operations:

* `get`: Provide a number which is not assigned to anyone.
* `check`: Check if a number is available or not.
* `release`: Recycle or release a number.



## 题目解读

&emsp;设计一个电话目录系统，支持分配号码、验证号码是否可分配、释放号码三个功能。号码有最大限制。题目对怎么分配号码没有详细说明，需要与面试官沟通确定（非常重要）。

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

* 用链表来记录分配过的号码，对于释放的号码不需要删除，将有效标识置为无效即可。则结合上面分配规则可得到代码。经过个人测试，目前的分配规则为：当历史分配号码不超过最大号码时，递增继续分配；当分配号码达到最大号码限制时，如果最大号码是释放过得，则分配这个最大号码，如果是有效的，则从前面寻找最小的释放号码，分配给这个号码。

> 上述初步分析出的分配规则是错的，具体见下面。

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

* 上述方法可以通过测试用例，但在调试时还是会发现与预期不相符的样例。经过进一步的分析，得出分配规则为：下一个待分配结点不为空则分配，结点后移；释放结点接拼接到下一个待分配结点后，如果下一个待分配结点为空，则这个释放的结点就是下一个待分配点。如果连续释放结点，由上面分析可知，最后释放的结点在下一个分配结点后面，最先释放反倒最先分配。可见这是一个完全的链表问题。
* 以最大号码为5为例，则初始链表为$0 \to 1 \to 2 \to 3 \to 4$，待分配结点指向0；连续分配四次，此时待分配结点指向4；如果此时释放号码1、3，则此时链表为$4 \to 3 \to1$，即1、3分别插入到4后面，下次分配从4开始，以此类推。若还是上述链表，连续分配五次，此时分配结点指向null，释放结点4、3，此时链表是$4 \to 3$，下次分配从4开始。

```java
class PhoneDirectory {
    // 记录是否在在使用
    private boolean[] phoneDir;
    // 哑结点指向待分配结点
    private Node dummy;

    /** Initialize your data structure here
        @param maxNumbers - The maximum numbers that can be stored in the phone directory. */
    public PhoneDirectory(int maxNumbers) {
        phoneDir = new boolean[maxNumbers];
        dummy = new Node(-1);
        Node tail = dummy;
        // 初始化链表
        for (int i = 0; i < maxNumbers; i++) {
            tail.next = new Node(i);
            tail = tail.next;
        }
    }
    
    /** Provide a number which is not assigned to anyone.
        @return - Return an available number. Return -1 if none is available. */
    public int get() {
        // 待分配结点为空
        if (dummy.next==null) {
            return -1;
        }
        // 分配结点
        int val = dummy.next.val;
        // 待分配指针指向下一个结点
        dummy.next = dummy.next.next;
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
        // 无效号码不做操作
        if(number >= phoneDir.length) {
            return;
        }
        // 已经释放过或没分配过，返回
        if (!phoneDir[number]) {
            return;
        }
        // 置为释放
        phoneDir[number] = false;
        // 待分配号码为空，直接拼接
        if(dummy.next == null) {
            dummy.next = new Node(number);
        }
        // 待分配号码不为空，则拼接在待分配号码后面
        else {
            Node temp = new Node(number);
            temp.next = dummy.next.next;
            dummy.next.next = temp;
        } 
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

测试样例：重点测试尾结点，当尾结点为最大号码时，尾结点有效和无效的情况下测试分配机制。

## 性能分析

&emsp;第一个设计三个方法在最糟糕的情况下，时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：62ms，在所有java提交中击败了16.67%的用户。

内存消耗：45.5MB，在所有java提交中击败了14.47%的用户。

&emsp;第二个设计由于在构造函数中完成了链表的创建，三个操作时间复杂度为$O(1)$，空间复杂度为$O(N)$。

执行用时：8ms，在所有java提交中击败了100.00%的用户。

内存消耗：39.3MB，在所有java提交中击败了89.47%的用户。

## 官方解题

&emsp;暂无，密切关系。

> 该题没有说明分配规则，也没有说明代码性能要求，需要与面试官沟通确认。
