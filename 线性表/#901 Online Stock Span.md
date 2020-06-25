[toc]

Write a class `StockSpanner` which collects daily price quotes for some stock, and returns the span of that stock's price for the current day.

The span of the stock's price today is defined as the maximum number of consecutive days (starting from today and going backwards) for which the price of the stock was less than or equal to today's price.

For example, if the price of a stock over the next 7 days were `[100, 80, 60, 70, 60, 75, 85]`, then the stock spans would be `[1, 1, 1, 2, 1, 4, 6]`.



**Note**:

* Calls to `StockSpanner.next(int price)` will have $1 \le \text{price} \le 10^5$.
* There will be at most `10000` calls to `StockSpanner.next` per test case.
* There will be at most `150000` calls to `StockSpanner.next` across all test cases.
* The total time limit for this problem has been reduced by 75% for C++, and 50% for all other languages.



## 题目解读

&emsp;返回股票当日价格的跨度。当天股票价格的跨度被定义为股票价格小于或等于今天价格的最大连续日数。

```java
class StockSpanner {

    public StockSpanner() {

    }
    
    public int next(int price) {

    }
}

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner obj = new StockSpanner();
 * int param_1 = obj.next(price);
 */
```

## 程序设计

* 设计链表结构，`pre`指向大于当前股票的节点，这样`pre`节点保存了之前插入节点的较大值，并维持一个递增序列；每次插入节点只需遍历`pre`指针并统计数目。

```java
class StockSpanner {
    Node tail;

    public StockSpanner() {
        this.tail = new Node(-1, 0, null);
    }
    
    public int next(int price) {
        Node cur = new Node(price, 1, tail);
        Node temp = tail;
        tail = tail.next = cur;
        
        while (temp.price != -1 && temp.price <= price) {
            cur.count += temp.count;
            temp = temp.pre;
        }
        cur.pre = temp;
        return cur.count;
    }
}

class Node {
    int price;
    int count;

    // 指向前面大于当前股价的节点
    Node pre;
    // 后继节点
    Node next;

    Node(int price, int count, Node pre) {
        this.price = price;
        this.count = count;
        this.pre = pre;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(N)$。

执行用时：27ms，在所有java提交中击败了99.06%的用户。

内存消耗：48.5MB，在所有java提交中击败了100.00%的用户。

## 官方解题

&emsp;官方采用栈来保存递减序列，但每次都要出栈入栈。社区时间性能较高的方法思路类似，采用两个数组，一个记录价格一个记录天数，组合实现跳跃链表思路。