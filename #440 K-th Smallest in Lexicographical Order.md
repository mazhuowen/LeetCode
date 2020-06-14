[toc]

Given integers `n` and `k`, find the lexicographically k-th smallest integer in the range from `1` to `n`.

Note: $1 \le k \le n \le 10^9$.



## 题目解读

&emsp;找到要求字典序的数字。

```java
class Solution {
    public int findKthNumber(int n, int k) {

    }
}
```

## 程序设计

* 

```java
class Solution {
    public int findKthNumber(int n, int k) {
        if (n < 1 || k > n) throw new IllegalArgumentException("invalid param");
        if (n < 10) return k;

        // 最高位和位数
        int top = n, d = 1;
        while (top > 10) {
            top /= 10;
            d++;
        }

        // 尝试最高位curTop的数字是否是第k个
        int curTop = 1;
        while (curTop <= 9) {
            int count = getCount(curTop, d, top, n);
            // 第k个数字起始数字找到
            if (k <= count) break;

            // 继续迭代
            k -= count;
            curTop++;
        }

        // 如果k低于位数，返回
        if (k <= d) return curTop * (int)Math.pow(10, k - 1); 

        int newN, temp = (int)Math.pow(10, d - 1);
        if (curTop != top) newN = n - temp;
        else newN = temp - 1;

        return curTop * temp + findKthNumber(newN, k - d);
    }

    // 获取以num开头的数字数目
    private int getCount(int num, int d, int top, int n) {
        int temp = (int)Math.pow(10, d - 1);
        // d-1位组成的数字数目
        int count = temp / 9;
        // d位组成的数字数目
        if (num == top) count += n - num * temp + 1;
        else if (num * temp < n) count += temp;

        return count;
    }
}
```

## 性能分析

&emsp;



## 官方解题

&emsp;