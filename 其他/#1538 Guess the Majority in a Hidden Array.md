[toc]

We have an integer array `nums`, where all the integers in `nums` are $0$ or $1$. You will not be given direct access to the array, instead, you will have an **API** `ArrayReader` which have the following functions:

* `int query(int a, int b, int c, int d)`: where $0 \le a < b < c < d < \text{ArrayReader.length()}$. The function returns the distribution of the value of the 4 elements and returns:

  * $4$ : if the values of the 4 elements are the same (0 or 1).
  * $2$ : if three elements have a value equal to 0 and one element has value equal to 1 or vice versa.
  * $0$ : if two element have a value equal to 0 and two elements have a value equal to 1.
* `int length()`: Returns the size of the array.

You are allowed to call `query()` $2 * n$ times at most where n is equal to `ArrayReader.length()`.

Return **any** index of the most frequent value in `nums`, in case of tie, return $-1$.

**Follow up**: What is the minimum number of calls needed to find the majority element?



**Constraints:**

- $5 \le \text{nums.length} \le 10^5$
- $0 \le \text{nums[i]} \le 1$



## 题目解读

&emsp;调用API猜测数组中最多的值，返回任意一个该值的索引。

```java
/**
 * // This is the ArrayReader's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface ArrayReader {
 *   public:
 *     // Compares 4 different elements in the array
 *     // return 4 if the values of the 4 elements are the same (0 or 1).
 *     // return 2 if three elements have a value equal to 0 and one element has value equal to 1 or vice versa.
 *     // return 0 : if two element have a value equal to 0 and two elements have a value equal to 1.
 *     public int query(int a, int b, int c, int d);
 *
 *     // Returns the length of the array
 *     public int length();
 * };
 */

class Solution {
    public int guessMajority(ArrayReader reader) {
        
    }
}
```

## 程序设计

* 基本思路是选择一个基准值，如索引$0$，固定$1$、$2$、$3$三个值，第四个值从$4$开始遍历，根据`query(0,1,2,3)`与`query(1,2,3,x)`的不同对两类数值计数；
* 最后就是判断$1$，$2$，$3$的类别，由于在遍历对比的过程中可以确定类别二的基准，再次调用可以判断这三类的类别计数差值。

```java
class Solution {
    public int guessMajority(ArrayReader reader) {
        int n = reader.length();
        // 两种符号的索引
        int type1 = 0, type2 = n - 1, count1 = 1, count2 = 0;
        int a = 1, b = 2, c = 3, d = n - 1;
        // 固定a、b、c，确定另一类
        int res = reader.query(type1, a, b, c);
        while (d > c) {
            // 和type1是同一类
            if (res == reader.query(a, b, c, d)) count1++;
            // 和type2是同一类
            else {
                type2 = d;
                count2++;
            }
            d--;
        }
        // 最后判断abc即可
        if (res == 4) count1 += 3;
        else if (res == 2) {
            // 三个值是type2
            if (reader.query(a, b, c, type2) == 4) count2 += 3;
            // 两个值是type1，一个是type2，相对值加一
            else count1 += 1;
        }
        // 两个值是type2，一个是type1，相对值加一
        else count2 += 1;

        if (count1 == count2) return -1;
        else if (count1 > count2) return type1;
        else return type2;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N)$，空间复杂度为$O(1)$。

执行用时：6ms，在所有java提交中击败了83.33%的用户。

内存消耗：45.7MB，在所有java提交中击败了16.67%的用户。

## 官方解题

&emsp;暂无，密切关注。可继续优化上述思路，由于前三个中必然存在两个相等的数，将这两个相等的数作为基准，迭代检测另两个数，统计计数；需注意最后可能剩余一个数未检测，可利用基准索引及另一类索引来检测，如果另一类索引截止目前都不存在，那结果必然是当前类，不必检测。

```java
class Solution {
    public int guessMajority(ArrayReader reader) {
        int n = reader.length();
        int count1 = 0, count2 = 0;
        // 相等的两个基准索引和另一类索引
        int base1 = 0, base2 = 1, base3 = -2, a = 2, b = 3;
        int res = reader.query(base1, 2, 3, 4);
        // 两个基准点相等，设为count1
        if (res == reader.query(base2, 2, 3, 4)) count1 = 2;
        else {
            if (reader.query(base1, 1, 3, 4) == reader.query(1, 2, 3, 4)) {
                base2 = 2;
                base3 = 1;
            } else {
                base1 = 1;
                base2 = 2;
                base3 = 0;
            }
            a = 3;
            b = 4;
            count1 = 1;
        }

        for (; b < n; a += 2, b += 2) {
            res = reader.query(base1, base2, a, b);
            // 全为类别一
            if (res == 4) count1 += 2;
            // 剩余两个值是另一个类
            else if (res == 0) {
                count2 += 2;
                base3 = b;
            }
            // 剩余两个数值类别各一半，计数抵消
        }

        // n-1位置未检测（如果n-1位置未检测且另一类在之前都没出现，则不必检测，结果必然是基准类）
        if (base3 != -2 && b == n) {
            if (base3 > base2) res = reader.query(base1, base2, base3, n - 1);
            else if (base3 > base1) res = reader.query(base1, base3, base2, n - 1);
            else res = reader.query(base3, base1, base2, n - 1);
            
            if (res == 2) count1++;
            else if (res == 0) count2++;
        }

        if (count1 == count2) return -1;
        else if (count1 > count2) return base1;
        else return base3;
    }
}
```

&emsp;时间复杂度为$O(\frac{N}{2})$，空间复杂度为$O(1)$。

执行用时：4ms，在所有java提交中击败了100.00%的用户。

内存消耗：44MB，在所有java提交中击败了83.33%的用户。