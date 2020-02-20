[toc]

Given an array of citations **sorted in ascending order** (each citation is a non-negative integer) of a researcher, write a function to compute the researcher's h-index.

According to the definition of h-index on Wikipedia: "A scientist has index h if h of his/her N papers have at least h citations each, and the other N − h papers have no more than h citations each."



Note:

If there are several possible values for $h$, the maximum one is taken as the h-index.

Follow up:

* This is a follow up problem to H-Index, where citations is now guaranteed to be sorted in ascending order.
* Could you solve it in logarithmic time complexity?



## 题目解读

&emsp;给定一位研究者论文被引用次数的数组（被引用次数是非负整数），数组已经按照升序排列，得出h指数。h代表**高引用次数（high citations）**，一名科研人员的h指数是指N篇论文中**至多有h篇论文分别被引用了至少h次**。

```java
class Solution {
    public int hIndex(int[] citations) {
        
    }
}
```

## 程序设计

* 文章i的引用次数为c，其后的文章数（加上i本身）是n-i；根据h指数的含义，n-i篇文章的引用大于等于n-i，由于数组是增序，只要c大于等于n-i，则n-i是一个合理值。
* 题目要求有多个h时选择最大的，实质上就是从左遍历数组，遇到的第一个满足条件的数就是最终h。

```java
class Solution {
    public int hIndex(int[] citations) {
        for(int i = 0; i < citations.length; i++) {
            // 遇到第一个满足条件的值，返回
            if(citations[i] >= citations.length - i) {
                return citations.length - i;
            }
        }
        // 未找到符合要求的条件，返回0
        return 0;
    }
}
```

* 采用二分查找，可以使用二分查找查找返回第一个c大于n-i的位置，代码部分如下：

```java
while(left < right) {
    int mid = (left + right) / 2;
    // 找到满足要求的h
    if(citations[mid] == (citations.length - mid)) {
        return citations.length - mid;
    }
     // mid的引用大于后面的文章数，向前找
     else if(citations[mid] > (citations.length - mid)) {
         // right保持mid，即保证区间里有至少一个c大于n-i
         right = mid;
     } 
    // mid文章的引用小于后面的文章数，向后找
    else {
        left = mid + 1;
    }
}
```

* 上述代码有个问题，就是需要额外处理没存在c大于n-i的情况，因为不存在c大于n-i时，上述代码无法工作。可以转变思路，right记录c大于n-i前的位置，这样不存在的情况下，right就是数组尾，left结束循环后在right后面，也就是n，得到的结果就是0；存在的情况下，left结束循环后在right后面，也就是第一个c大于n-i前的位置。代码修改如下：

```java
class Solution {
    public int hIndex(int[] citations) {
        int left = 0, right = citations.length - 1;
        while(left <= right) {
            int mid = (left + right) / 2;
            // 找到满足要求的h，此h就是最大的h
            if(citations[mid] == (citations.length - mid)) {
                return citations.length - mid;
            }
            // mid文章的引用大于后面的文章数，向前找，直到right保持为citations[mid] < (citations.length - mid)
            else if(citations[mid] > (citations.length - mid)) {
                right = mid - 1;
            } 
            // mid文章的引用大于后面的文章数，向后找
            else {
                left = mid + 1;
            }
        }
        // 关键，循环中right最终会保持在citations[mid] > citations.length - mid（如果存在）前的citations[mid] < citations.length - mid的位置；不存在则right为表尾。
        // left结束循环后在right后面，要么是第一个citations[mid] > citations.length - mid的位置，要么是n
        return citations.length - left;
    }
}
```

## 性能分析

&emsp;顺序遍历时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：1ms，在所有 Java 提交中击败了30.41%的用户。

内存消耗：44.9MB，在所有java提交中击败了19.69%的用户。

&emsp;时间复杂度为$O(\log_2N)$，空间复杂度为$O(1)$。

执行用时：0ms，在所有java提交中击败了100.00%的用户。

内存消耗：45.4MB，在所有java提交中击败了5.51%的用户。

## 官方解题

&emsp;参见上面二分查找。