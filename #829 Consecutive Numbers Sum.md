[toc]





## 题目解读

&emsp;

```java

```

## 程序设计

* 

```java
class Solution {
    public int consecutiveNumbersSum(int N) {
        if (N < 1) throw new IllegalArgumentException("invalid param");

        int count = 0;
        int maxLen = (int)(Math.sqrt(2 * N + 0.25) - 0.5);

        for (int len = 1; len <= maxLen; len++) {
            int val = 2 * N - len * len + len;
            if (val < 1 || val % (2 * len) != 0) continue;
            count++;
        }
        return count;
    }
}
```

## 性能分析

&emsp;



## 官方解题

&emsp;