[toc]

## 顺序表

### 基本操作

* 简单[#1 Two Sum](./#1 Two Sum.md)   顺序表遍历
* $\clubs$技巧[#26 Remove Duplicates from Sorted Arra](./#26 Remove Duplicates from Sorted Array.md)    有序顺序表的技巧删除
* $\clubs$技巧[#27 Remove Element](./#27 Remove Element.md)    顺序表的技巧删除
* 简单[#80 Remove Duplicates from Sorted Array II](./#80 Remove Duplicates from Sorted Array II.md)    数组移动覆盖
* 中等[#163 Missing Ranges](./#163 Missing Ranges.md)    数组遍历比较相邻值
* 简单[#228 Summary Ranges](./#228 Summary Ranges.md)    连续数值区间判断
* 简单[#243 Shortest Word Distance](./#243 Shortest Word Distance.md)    数组遍历比较
* 中等[#245 Shortest Word Distance III](./#245 Shortest Word Distance III.md)    [#243 Shortest Word Distance](./#243 Shortest Word Distance.md)进阶
* 中等[#900 RLE Iterator](./#900 RLE Iterator.md)    数组计数迭代

### 双指针

* 技巧[#11 Container With Most Water](./#11 Container With Most Water.md)    双指针
* 中等[#15 3Sum](./#15 3Sum.md)    双指针
* 中等[#16 3Sum Closest](./#16 3Sum Closest.md)    [#15 3Sum](./#15 3Sum.md)的变体
* $\bigstar$中等[#18 4Sum](./#18 4Sum.md)    [#15 3Sum](./#15 3Sum.md)的延续
* 简单[#121 Best Time to Buy and Sell Stock](./#121 Best Time to Buy and Sell Stock.md)    遍历记录区间最大值最小值
* 中等[#360 Sort Transformed Array](./#360 Sort Transformed Array.md)    先计算再双指针遍历
* $\bigstar$中等[#457 Circular Array Loop](./#457 Circular Array Loop.md)    快慢指针检测环
* 简单[#849 Maximize Distance to Closest Person](./#849 Maximize Distance to Closest Person.md)    双指针记录非空位置
* 中等[#986 Interval List Intersections](./#986 Interval List Intersections.md)    双指针遍历两个数组
* 中等[#1229 Meeting Scheduler](./#1229 Meeting Scheduler.md)    双指针遍历迭代数组

### 数组交换

* $\bigstar$中等[#189 Rotate Array](./#189 Rotate Array.md)    循环移位
* 简单[#283 Move Zeroes](./#283 Move Zeroes.md)    交换0到最后
* 简单[#832 Flipping an Image](./#832 Flipping an Image.md)    交换并翻转值

## 链表

&emsp;链表的基本操作有插入、删除、遍历；进阶的有链表反转、子链表的操作；在这些基础上又衍生出循环链表的判断、相交链表的判断。在这些操作中，普遍使用哑结点、双指针等技巧简化特殊情况。

&emsp;插入需要知道待插入位置的前驱结点，对于链表$A \to B$，插入$C$，则需要直到前驱结点$A$，`C.next = A.next`操作使得$C \to B$，此时$A$是断开的，`A.next = C`连接$A$、$C$完成插入。对于特定位置的插入分为头插法、尾插法。对于头插法，假定当前链表为$A \to \dots$，指针`head`指向$A$，若要在头部插入$B$，则`B.next = head`插入到表头，然后更新头指针`head = B`；对于尾插法，假设当前链表为$\dots \to A$，指针`tail`指向结点$A$，当插入$B$时，`tail.next = B`，更新尾结点`tail = B`。这些插入操作可以实现链表很多功能。

&emsp;删除操作也需要知道前驱结点，对于链表$A \to B \to C$，`A.next = A.next.next`删除结点$B$。头结点的删除只需要更新头结点`head = head.next`。对于一些不对结点结构作要求，只重视值的情况，只需知道待删除结点即可，将其后继结点值复制到待删除结点，然后删除后继结点，等效的删除了当前结点，因为当前结点的值不存在了。

&emsp;遍历操作顺着链表遍历结点，同样分为两种。一种是根据当前结点遍历：

```java
while(head != null) {
    head = head.next;
}
```

上述方法常用于统计链表长度等不依赖前驱结点的操作；另一种是根据后继结点遍历：

```java
while(head.next != null) {
    head = head.next;
}
```

该方法遍历过程中可以提供前驱结点的信息，对于依赖前驱结点的操作，比如删除、插入等，可以在遍历的过程中操作。

&emsp;在这些基本操作上，链表反转就是遍历链表将遍历的结点通过头插法插入到标识结点：`insert.next = dummy.next`，`dummy.next = insert`，其中`insert`是待插入结点，`dummy`是标识结点，对于反转整个链表其就是新建的哑结点，对于翻转子链表其就是子链表的前驱结点。特别的，对于反转$pre \to A \to \dots \to B \to next$链表中的$A \to \dots \to B$子链表，从$A$的后继进行遍历：

```java
temp = A;
while(A.next != next) {
    insert = A.next;
    // 删除A的后继
    A.next = insert.next;
    // 头插法
    insert.next = pre.next;
    pre.next = insert;
}
```

这样在遍历的过程中完成链表反转，前提是要知道前驱结点$pre$和后继结点$next$。除了这种子链表整体反转连接，还有只需反转子链表指向的操作，即$A \to B \to C \to D$，将后半部分子链表反转为$A \to B \to C \gets D$，需要在遍历时保存前驱、当前结点、后继结点三个结点信息，改变当前结点指向前驱`D.next = C`（第一个前驱结点还要取消指向后继的操作，避免形成循环链表`C.next = null`），而保存后继结点是为了继续迭代，重复这个过程。

&emsp;循环链表、相交链表都需要用到双指针。循环链表可以利用快慢指针确认是否存在环，进一步可以确认环在链表上的第一个结点；相交链表可利用两个链表同时遍历确认各自表长，然后通过快慢指针判断是否相交并得到相交点。双指针除了上述应用，还可以确定链表中间结点，或者执行链表的窗口操作，总之双指针应用及其灵活，需要多发挥想象力。

### 链表的遍历

* 中等[#2 Add Two Numbers](./#2 Add Two Numbers.md)    两个链表同时遍历运算
* $\clubs$简单[#21 Merge Two Sorted Lists](./#21 Merge Two Sorted Lists.md)    逐个遍历比较拼接
* $\bigstar$技巧[#23 Merge k Sorted Lists](./#23 Merge k Sorted Lists.md)    分治法，最后化为两个链表的合并
* 简单[#203 Remove Linked List Elements](./#203 Remove Linked List Elements.md)    简单遍历删除操作
* 简单[#328 Odd Even Linked List](./#328 Odd Even Linked List.md)    双指针遍历形成两链表，合并得到结果
* 中等[#430 Flatten a Multilevel Doubly Linked List](./#430 Flatten a Multilevel Doubly Linked List.md)    多层双向链表遍历拼接为单层
* $\clubs$中等[#708 Insert into a Sorted Circular Linked List](./#708 Insert into a Sorted Circular Linked List.md)    表头非最小值的循环链表遍历插入
* 中等[#725 Split Linked List in Parts](./#725 Split Linked List in Parts.md)    链表的遍历重组
* $\clubs$技巧[#817 Linked List Components](./#817 Linked List Components.md)    链表连续序列的判断
* 中等[#901 Online Stock Span](./#901 Online Stock Span.md)    跳跃链表
* 中等[#面试题02.05 Sum Lists LCCI](./#面试题02.05 Sum Lists LCCI.md)    链表遍历求和

### 链表的插入删除

* $\clubs$简单[#24 Swap Nodes in Pairs](./#24 Swap Nodes in Pairs.md)    删除插入操作交换相邻结点位置，遍历迭代
* $\bigstar$困难[#25 Reverse Nodes in k-Group](./#25 Reverse Nodes in k-Group.md)    双指针遍历分割$k$子链表，翻转并拼接
* $\bigstar$中等[#82 Remove Duplicates from Sorted List II](./#82 Remove Duplicates from Sorted List II.md)    双指针遍历比较，子链表删除
* 简单[#83 Remove Duplicates from Sorted List](./#83 Remove Duplicates from Sorted List.md)    遍历结点，删除相同值的后继节点
* 中等[#86 Partition List](./#86 Partition List.md)    删除后半段小于阈值的结点，插入到前半部分链表
* $\clubs$中等[#146 LRU Cache](./#146 LRU Cache.md)    链表与哈希表结合实现`LRU`
* 简单[#237 Delete Node in a Linked List](./#237 Delete Node in a Linked List.md)    只给定待删除结点，复制后继结点值删除
* $\clubs$技巧[#379 Design Phone Directory](./#379 Design Phone Directory.md)    基于特殊规则的插入操作
* 繁杂[#432 All O`one Data Structure](./#432 All O`one Data Structure.md)    哈希表记录维护链表有序性
* 中等[#707 Design Linked List](./#707 Design Linked List.md)    链表类的设计
* $\clubs$中等[#1171 Remove Zero Sum Consecutive Nodes from Linked List](./#1171 Remove Zero Sum Consecutive Nodes from Linked List.md)    前缀和在链表中的应用

### 链表的反转

* $\clubs$经典[#92 Reverse Linked List II](./#92 Reverse Linked List II.md)    反转指定子段的子链表
* 简单[#206 Reverse Linked List](./#206 Reverse Linked List.md)    反转整个链表，注意递归实现
* $\clubs$技巧[#234 Palindrome Linked List](./#234 Palindrome Linked List.md)    回文判断，双指针查找中点，反转后半段
* 中等[#445 Add Two Numbers II](./#445 Add Two Numbers II.md)    链表翻转计算

### 循环链表

* 简单[#141 Linked List Cycle](./#141 Linked List Cycle.md)
* $\bigstar$技巧[#142 Linked List Cycle II](./#142 Linked List Cycle II.md)    快慢指针确定环的存在，并找到环起始点

### 双指针

* $\bigstar$技巧[#19 Remove Nth Node From End of List](./#19 Remove Nth Node From End of List.md)    双指针定位倒数结点
* $\clubs$技巧[#61 Rotate List](./#61 Rotate List.md)    双指针定位倒数链表，并拼接到链表头
* $\clubs$简单[#160 Intersection of Two Linked Lists](./#160 Intersection of Two Linked Lists.md)    双指针判断相交结点
* 中等[#143 Reorder List](./#143 Reorder List.md)    双指针定位中间结点，反转后半段并合并
* 简单[#369 Plus One Linked List](./#369 Plus One Linked List.md)    递归或双指针确定子链表进行加一操作

### 交叉领域

* $\clubs$中等-二叉查找树[#109 Convert Sorted List to Binary Search Tree](./#109 Convert Sorted List to Binary Search Tree.md)    模拟中序遍历过程构建树
* $\clubs$中等-插入排序[#147 Insertion Sort List](./#147 Insertion Sort List.md)    链表插入排序实现，结点删除插入遍历
* $\bigstar$繁杂-归并排序[#148 Sort List](./#148 Sort List.md)    自底向上的归并排序实现，边界条件繁杂

### 进阶

* $\bigstar$技巧[#138 Copy List with Random Pointer](./#138 Copy List with Random Pointer.md)    灵活应用链表结构，拷贝随机结点

