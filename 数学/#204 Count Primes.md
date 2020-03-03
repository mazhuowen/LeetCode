[toc]

Count the number of prime numbers less than a non-negative number, **n**.



## 题目解读

&emsp;统计所有小于非负整数n的质数的数量。

```java
class Solution {
    public int countPrimes(int n) {

    }
}
```

## 程序设计

* 最基本的思路是逐个检查，记录之前的质数，如果当前数能被之前的质数整除，则不是质数，否则计数加一。但是该方法会报时间超时。

```java
class Solution {
    public int countPrimes(int n) {
        if(n < 2) {
            return 0;
        }
        boolean[] dp = new boolean[n < 7 ? 8 : n + 1];
        dp[2] = dp[3] = dp[5] = dp[7] = true;
        int count = 0;
        for(int i = 2; i < n; i++) {
            boolean flag = true;
            for(int j = 2; j <= Math.sqrt(i); j++) {
                // 能被其它质数整除，不符合要求，退出
                if(dp[j] && i % j == 0) {
                    flag = false;
                    break;
                }
            }
            // 当前数字是质数
            if(flag) {
                count++;
                dp[i] = true;
            }
        }
        return count;
    }
}
```

* 上述思路采用数组保存质数及非质数，每次需要遍历，可以使用链表直接保存质数，修改如下：

```java
class Solution {
    public int countPrimes(int n) {
        if(n < 2) {
            return 0;
        }
        List<Integer> primes = new ArrayList<>();
        primes.add(2); primes.add(3); primes.add(5); primes.add(7);
        int count = 0;
        for(int i = 2; i < n; i++) {
            boolean flag = true;
            for(int j = 0; primes.get(j) <= Math.sqrt(i); j++) {
                // 能被其它质数整除，不符合要求，退出
                if(i % primes.get(j) == 0) {
                    flag = false;
                    break;
                }
            }
            // 当前数字是质数
            if(flag) {
                count++;
                primes.add(i);
            }
        }
        return count;
    }
}
```

## 性能分析

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：378ms，在所有java提交中击败了11.46%的用户。

内存消耗：41.3MB，在所有java提交中击败了5.07%的用户。

## 官方解题

&emsp;暂无，密切关注。社区中时间性能较快的方法采用将当前质数的倍数标记为非质数的方法。

```java
class Solution {
    public int countPrimes(int n) {
        if(n < 2) {
            return 0;
        }
        boolean[] dp = new boolean[n];
        double stop = Math.sqrt(n);
        for(int i = 2; i <= stop; i++) {
            // 当前是非质数，继续迭代（不更新是为了避免重复更新，浪费时间）
            if(dp[i])
                continue;
            // j可以被i乘除，不是质数
            for(int j = i << 1; j < n; j += i) {
                dp[j] = true;
            }
        }
        int count = 0;
        for(int i = 2; i < n; i++) {
            if(!dp[i]) count++;
        }
        return count;
    }
}
```

&emsp;时间复杂度为$O(N^2)$，空间复杂度为$O(N)$。

执行用时：14ms，在所有java提交中击败了78.94%的用户。

内存消耗：39.4MB，在所有java提交中击败了16.08%的用户。

> 时间性能大幅提高主要原因是取消取余操作，采用简单的加法操作。