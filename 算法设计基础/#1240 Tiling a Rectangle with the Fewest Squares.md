[toc]

Given a rectangle of size $n \times m$, find the minimum number of integer-sided squares that tile the rectangle.



**Constraints:**

- $1 \le n \le 13$
- $1 \le m \le 13$



## 题目解读

&emsp;给定矩形，输出填充矩形最少的正方形数目。

```java
class Solution {
    public int tilingRectangle(int n, int m) {

    }
}
```

## 程序设计



```java
class Solution {
    public int tilingRectangle(int n, int m) {
        if (n == m) return 1;
        if (n > m) return 1 + tilingRectangle(n - m, m);
        else return 1 + tilingRectangle(m - n, n);
    }
}
```



<img src="../images/#1240.png" style="zoom: 50%;" />



## 性能分析



## 官方解题